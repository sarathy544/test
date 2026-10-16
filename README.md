# OneApp OneMeet — Step 1 Repository Architecture Audit

**Audit date:** 2026-10-16  
**Input:** `Project_Maker.zip`  
**Project inspected:** `One Browser/` (OneApp OneMeet Electron application)  
**Audit type:** Static source review + JavaScript syntax checks  
**Status:** Step 1 complete; no application source files modified.

## 1. Executive Summary

The uploaded archive contains the OneApp OneMeet Electron project under `One Browser/`. The project already has a useful foundation for the planned LAN architecture:

- An HTTP server (`src/main/host.js`) serves the shell, inserted modules, and module APIs.
- The server can bind to loopback or all IPv4 interfaces depending on Network settings.
- Modules can declare an optional server file in `module.json` and register module API routes.
- The shell has a centralized API helper (`src/shell/js/api.js`) and shared UI/table helpers.
- A browser fallback SDK exists (`sdk/one-browser.js`), while Electron-native capabilities are exposed through a restricted preload bridge.
- The current architecture is still Electron-coupled: `host.js` imports `config.js` and `modules.js`, both of which import Electron's `app` object. Therefore, the current server is **not yet a standalone headless server**.

The best next step is a **small, reversible refactor**, not a rewrite: separate platform-neutral server/config/module services from Electron startup and native integrations, then prove the headless server on the existing Windows PC before choosing Mini PC/Raspberry Pi hardware.

## 2. Archive and Project Structure

The ZIP is approximately 349 MB compressed and about 955 MB uncompressed. It includes `node_modules`, `.git`, multiple historical Android APKs, Android build output, and a file named `Phone App/source/app.keystore`. These are not needed for a source-only architecture review and make the archive unnecessarily large.

Main OneApp project source paths found:

```text
One Browser/
├── package.json
├── package-lock.json
├── README.md
├── sdk/
│   └── one-browser.js
├── examples/
│   └── test-app/
│       ├── index.html
│       ├── invoice.html
│       └── server.js
└── src/
    ├── main/
    │   ├── main.js
    │   ├── host.js
    │   ├── modules.js
    │   ├── config.js
    │   ├── connect.js
    │   ├── lan.js
    │   └── bridge/
    │       ├── index.js
    │       ├── devices.js
    │       ├── files.js
    │       ├── print.js
    │       ├── send.js
    │       └── system.js
    ├── preload/
    │   └── preload.js
    ├── shell/
    │   ├── index.html
    │   ├── unlock.html
    │   ├── css/
    │   │   ├── app.css
    │   │   ├── onemeet.css
    │   │   └── table.css
    │   ├── js/
    │   │   ├── api.js
    │   │   ├── icons.js
    │   │   ├── qr.js
    │   │   ├── shell.js
    │   │   ├── table.js
    │   │   ├── theme.js
    │   │   ├── ui.js
    │   │   └── screens/
    │   │       ├── dashboard.js
    │   │       └── settings.js
    │   └── vendor/
    │       └── AGScriptLike.js
    └── ui/
        ├── pdf-print.html
        └── style.css
```

No `module.json` business modules for Machine Registry, Basic Invoice Entry, Stock/Inventory, or Parts Requirement were present inside the `One Browser/` project source in this ZIP. The README refers to external `Project_Maker\\Module\\...` folders, which were not included as One Browser module folders in the inspected source. The actual module implementations and their data schemas therefore remain unaudited.

## 3. Current Runtime and Dependency Findings

### 3.1 `package.json`

Verified:
- Application entry: `src/main/main.js`.
- `npm start` launches Electron.
- Build scripts exist for Windows and Linux.
- Dependencies include `nodemailer` and `pdfjs-dist`; `serialport` is optional.
- There is no `test` script in `package.json`.

Implication: the current startup path is explicitly an Electron desktop application. A dedicated headless-server start command and server-specific deployment configuration do not yet exist in the inspected package scripts.

### 3.2 `src/main/main.js`

Verified:
- Creates the Electron `BrowserWindow`.
- Enables `contextIsolation`, disables `nodeIntegration`, and enables `sandbox`.
- Registers native bridge handlers and app/settings IPC.
- Starts `host.start()` inside `app.whenReady()`.
- Stops the host server on Electron `before-quit`.
- Uses Electron `app.getPath('userData')`, BrowserWindow, dialogs, shell, and app lifecycle.

Implication: server startup is currently tied to Electron's lifecycle. A headless mode needs its own entry point and lifecycle handling.

### 3.3 `src/main/host.js`

Verified:
- Uses Node's built-in `http` server.
- Serves the shell, module assets, module pages/mounts, and module APIs.
- Provides `GET /api/shell/info`, `GET /api/shell/modules`, and `/api/m/:id/...` routing.
- Loads module server handlers from each module's `server` file.
- Binds to `127.0.0.1` by default, and `0.0.0.0` when network sharing is enabled and mode is `main`.
- Starts at the configured port or scans for a free port.
- Has a body size limit of 8 MB for JSON and 64 MB for non-JSON bodies.
- Has a PIN gate for remote clients when a PIN is configured.

Critical architecture coupling:
- `host.js` imports `./config` and `./modules`.
- `config.js` imports `{ app }` from `electron`.
- `modules.js` imports `{ app }` from `electron`.
- `host.js` also imports Electron in `info()` to obtain app version.

Therefore, moving `host.js` to a Mini PC without Electron will fail unless these dependencies are separated or abstracted.

### 3.4 `src/main/config.js` and `src/main/modules.js`

Verified:
- Configuration is stored in Electron's user-data folder.
- Module data is stored under `module-data/<id>`.
- Modules can be loaded from configured folders or copied into the app's own modules folder.
- Manifests support scripts, styles, server handlers, pages, mounts, aliases, dependency ordering, and enable/disable status.
- Module insertion copies module code into the app data directory and reloads module routes.

Implication:
- Preserve the existing module manifest model where practical.
- Add platform capability declarations only after defining a backward-compatible manifest extension.
- Extract path/config/data-root handling so headless mode can supply its own data directory without depending on Electron's `app`.

### 3.5 UI and SDK

Verified:
- `src/shell/index.html` loads the common styles, icons, theme, API, UI, table, QR, and shell scripts.
- Dashboard and Settings are built-in screens.
- The shell comments explicitly require module lists to use `EC.table(...)` rather than creating independent table instances.
- `src/shell/js/api.js` is intended to centralize calls to the server.
- `sdk/one-browser.js` uses `window.oneBrowser` inside Electron and has browser fallbacks for selected operations.
- `src/preload/preload.js` exposes a curated `window.oneBrowser` API through `contextBridge`.
- `src/main/bridge/index.js` checks the calling frame/origin before handling native IPC requests.

Implication:
- Keep shared UI behavior centralized.
- Browser modules must not rely on Electron-only bridge operations for their essential data-entry workflows.
- Continue to enforce the existing design rule that lists use `EC.table(...)`.

## 4. Security and Reliability Findings to Address

These are source-code observations and recommended follow-ups, not a claim that the app has been penetration-tested.

### High priority — LAN sharing without a configured PIN

`src/main/host.js` comments and logic show that remote access is unlocked when no PIN is configured. `config.js` defaults the PIN to an empty string. If sharing is enabled while the PIN remains empty, the server does not require a PIN cookie for remote requests.

Recommendation:
- For a LAN deployment with business data, require authentication before enabling shared write access, or make the user explicitly confirm the unauthenticated mode.
- Apply authorization to write routes, not just the shell page.
- Define roles/permissions for the four business modules.

### High priority — PIN storage and session design

Observed:
- The configured PIN is compared as a string.
- A deterministic SHA-256-derived value is used as the cookie token.
- The cookie is `HttpOnly` and `SameSite=Lax`, but it has a one-year max age and no `Secure` attribute in the shown code.
- No rate limiting is visible in the PIN unlock handler in `host.js`.

Recommendations:
- Use a proper authentication/session design rather than a long-lived deterministic PIN-derived cookie.
- Add rate limiting and backoff to unlock attempts.
- Use HTTPS and `Secure` cookies when the deployment supports HTTPS.
- Add session revocation/rotation and shorter, configurable expiry.
- Do not store reusable credentials in plaintext configuration if the authentication design can avoid it.
- Validate `Origin`/CSRF protections for state-changing browser requests where applicable.

### Medium priority — HTTP-only LAN transport

The current built-in host creates an HTTP server. On a trusted isolated LAN this may be a deliberate initial trade-off, but HTTP does not protect session cookies or business data against a network observer.

Recommendation:
- Decide whether the initial LAN deployment will use HTTPS with a manageable local certificate strategy.
- If HTTP is temporarily retained, document the threat model and do not describe it as encrypted or secure against LAN interception.
- Do not expose the port to the public Internet.

### Medium priority — Module code trust boundary

The module loader can copy module directories and load each module's server JavaScript with `require()`. This means inserting a module is a trusted-code operation, not merely installing passive UI files.

Recommendations:
- Treat module insertion as an administrative operation.
- Validate and review modules before enabling them.
- Document that server modules execute with the server process's permissions.
- Avoid allowing untrusted users to insert or edit server modules.
- Consider separating module API permissions from filesystem/native desktop permissions.

### Medium priority — Module API authorization

The host-level PIN gate is a coarse access gate. The shown `moduleApi()` dispatch does not itself define per-user role checks; module handlers receive request headers and body and must enforce domain rules as appropriate.

Recommendation:
- Add a shared authentication/authorization context to API handlers.
- Enforce permission checks server-side for machine, invoice, stock, and parts operations.
- Add input validation, idempotency/duplicate protection, and audit trails where required.

### Archive hygiene — Android signing keystore

The ZIP includes `Phone App/source/app.keystore`. The archive inspection did not read or disclose its contents. If this is a real release-signing keystore, treat the fact that it was bundled into the project archive as a key-management concern:
- Keep production signing keys outside source archives and repositories.
- Check whether the keystore and its password are already exposed elsewhere.
- If it is a production key and exposure is plausible, follow the appropriate Android signing-key incident/rotation plan.
- Do not include keystores, credentials, generated APKs, `node_modules`, or `.git` in future source-only audit ZIPs unless specifically needed.

## 5. Target Architecture Recommended for This Repository

Do not rewrite the existing application. Introduce a platform-neutral server layer while preserving the current Electron desktop behavior.

Proposed structure (illustrative; final paths depend on the refactor):

```text
src/
├── core/
│   ├── config/
│   │   └── config-store.js       # filesystem/environment based, no Electron import
│   ├── modules/
│   │   ├── manifest.js           # schema validation
│   │   ├── registry.js           # module discovery/ordering
│   │   └── loader.js             # trusted module API loading
│   ├── server/
│   │   ├── create-server.js      # HTTP routing and static assets
│   │   ├── middleware/
│   │   ├── auth/
│   │   └── health.js
│   └── data/
│       └── data-root.js
├── server/
│   └── headless.js               # starts server without Electron
├── main/
│   ├── main.js                   # Electron window/lifecycle
│   ├── config.js                 # adapter to Electron userData if retained
│   └── bridge/                   # desktop-only native operations
├── preload/
└── shell/
```

This is a design proposal, not a statement that these files already exist.

### Responsibility boundaries

- **Core server:** HTTP, API routing, module route dispatch, request parsing, shared error format, health endpoint.
- **Config/data adapters:** accept a configured data root; do not import Electron directly.
- **Headless entry:** Node.js startup, signal handling, logging, shutdown.
- **Electron entry:** window, menu, dialogs, native bridge, app lifecycle.
- **Business module/server handler:** business validation and data operations.
- **Browser UI:** API-driven data entry and shared UI components.
- **Desktop-only worker:** OCR/PDF/heavy AI and approved native functions.

### Data store

SQLite is a reasonable candidate for the initial 1–3-user deployment, but the uploaded One Browser source does not include the target business module schemas or a central business database implementation. Do not create a new schema until the existing `Project_Maker` module source and its database responsibilities have been inspected.

The server alone should own the live database file. Desktop and phone clients should access it through the API.

## 6. Implementation Plan After the Audit

### Phase A — Headless server proof of concept
1. Extract server logic that can run without Electron.
2. Make configuration and data-root paths injectable.
3. Add a Node-only server entry point.
4. Add `/health` or a versioned health endpoint.
5. Keep the existing Electron `host.start()` behavior working through the same core server.
6. Test both startup modes on Windows before deploying to Linux.

### Phase B — Security baseline
1. Define required authentication for LAN sharing.
2. Add server-side authorization hooks.
3. Add request validation and rate limiting.
4. Decide HTTP/HTTPS for the LAN deployment.
5. Verify the firewall only allows the intended private network.

### Phase C — Four lightweight business workflows
1. Machine Registry.
2. Stock / Inventory.
3. Parts Requirement.
4. Basic Invoice Entry.

For each, inspect the actual module and schema first, then connect the browser UI through the shared API. Preserve contextual record creation and unsaved form data on errors.

### Phase D — Desktop heavy processing
Keep OCR, PDF conversion, and heavy AI in the desktop environment. Add a queue/job API only if a phone-originated task needs to wait for a desktop worker.

### Phase E — Deployment verification
Test 1–3 concurrent users, no-Internet LAN operation, restart recovery, backup/restore, authorization, and resource use before choosing final hardware.

## 7. Static Validation Performed

- ZIP contents enumerated without extracting `node_modules` or `.git` into the audit source directory.
- 25 JavaScript source files under `One Browser/` were checked with `node --check`.
- Result: **25/25 JavaScript files passed syntax checking**.
- This does not prove runtime correctness or feature behavior.
- No automated test suite was identified in the project package scripts; `package.json` has no `test` script.
- No Electron runtime, browser integration, security penetration test, or hardware benchmark was performed.
- No source files were modified during this Step 1 audit.

## 8. Step 1 Completion and Next Step

**Step 1 — Repository Architecture Audit: COMPLETE.**

The main blocker for the requested Mini PC server is confirmed: the existing HTTP server is coupled to Electron-bound config and module services. The current project already has module APIs and a shared UI foundation that should be preserved.

**Next recommended step:** implement Phase A only — the minimal headless-server proof of concept — on a separate branch or as a reversible change. Do not yet implement the four business modules or change the database until their real source and schema are available.
