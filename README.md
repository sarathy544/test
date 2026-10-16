# OneApp OneMeet — Unified Project Constitution, UX Standards & LAN Server Architecture

Version: 2.0.0
Status: Proposed Project Standard — verify implementation details against source code before coding
Scope: Entire OneApp OneMeet Application
Applies To: AI Coding Agents, Developers, Shared Components, Modules, UI/UX, Testing and Maintenance

## 1. CORE OBJECTIVE

OneApp OneMeet must provide a consistent, simple, efficient, and maintainable user experience.

The application must minimize:
- Unnecessary user actions.
- Repeated data entry.
- Unnecessary screen navigation.
- Duplicate component implementations.
- Temporary UI workarounds.
- Confusing interaction patterns.
- Repeated development and redesign work.
- Regression defects caused by shared component changes.

The primary development principle is:

IDENTIFY THE GAP → ENHANCE THE RIGHT SHARED COMPONENT → TEST → REUSE → VALIDATE THE COMPLETE WORKFLOW.

A feature must not be implemented merely because it works. It must also follow the approved UX workflow and shared component architecture.

## 2. ARCHITECTURAL PRINCIPLES

### 2.1 Common Shell

The application has a common shell containing Dashboard and Settings, with additional screens supplied by modules.

The existing shell, shared styles, UI utilities, icon definitions, table implementation, and navigation conventions are authoritative references.

Modules must integrate with the common shell rather than independently recreating its functionality.

### 2.2 Single Source of Truth

Shared UI behavior must be implemented in the appropriate shared component or service.

Do not create multiple independent implementations of the same shared responsibility.

### 2.3 Reusable Component First

Before implementing a feature, inspect the existing reusable components and determine whether they already support the required behavior.

If a required capability is missing, identify the gap before writing module-specific implementation code.

### 2.4 Workflow First

Before choosing controls or writing code, understand the user's objective and define the simplest safe and understandable workflow.

Component selection must support the workflow rather than dictate an unnecessarily complicated workflow.

### 2.5 Controlled Component Evolution

Reusable components must evolve when legitimate, reusable requirements arise.

Enhancements must preserve existing behavior wherever practical and must not introduce unrelated responsibilities into shared components.

## 3. MANDATORY FEATURE DEVELOPMENT WORKFLOW

Every feature must follow this sequence.

STEP 1 — UNDERSTAND THE USER GOAL
Identify the task the user wants to complete and its expected outcome.

STEP 2 — DEFINE THE OPTIMIZED WORKFLOW
Identify the necessary user actions, dependencies, validations, and completion state.

STEP 3 — INSPECT EXISTING IMPLEMENTATION
Inspect existing screens, reusable components, APIs, styles, utilities, and module conventions.

STEP 4 — IDENTIFY CAPABILITY GAPS
Determine whether existing components can support the required workflow without duplication or architectural violations.

STEP 5 — CHOOSE THE CORRECT IMPLEMENTATION LEVEL
Choose one of the following:
- Reuse an existing component.
- Extend an existing shared component.
- Add a reusable companion component or shared service.
- Implement module-specific business logic when it belongs to that module.
- Introduce a new component only when existing abstractions are unsuitable.

STEP 6 — IMPLEMENT SHARED CAPABILITIES FIRST
When a shared capability is required, implement and validate that capability before completing the dependent feature.

STEP 7 — IMPLEMENT THE FEATURE
Use the approved component or service and follow the optimized UX workflow.

STEP 8 — TEST AND VERIFY
Run applicable component tests, integration tests, regression tests, and workflow checks.

STEP 9 — UPDATE DOCUMENTATION
Document new capabilities, public APIs, configuration options, supported states, and limitations.

STEP 10 — REPORT COMPLETION
Report the implementation, shared components changed, tests executed, and remaining limitations.

Do not skip capability-gap analysis merely to complete a feature faster.

## 4. REUSABLE COMPONENT EVOLUTION POLICY

### 4.1 Mandatory Capability Assessment

When an existing component cannot support a required workflow, the developer or AI agent must identify:

- The component's current capabilities.
- The missing capability.
- Why the existing API is insufficient.
- Whether the requirement belongs to the component.
- The proposed reusable solution.
- Backward-compatibility implications.
- Required tests.

### 4.2 Enhance Before Workaround

A module-specific workaround must not be the default response to a shared component limitation.

Prefer enhancing the appropriate shared component when the missing behavior is a legitimate, reusable responsibility.

Examples:
- Add supported creation behavior to a reusable searchable selector.
- Add missing editing capabilities to a reusable table.
- Add appropriate asynchronous submission states to a reusable form or dialog.
- Add reusable loading, empty, and error states where required.
- Add shared keyboard and accessibility behavior when appropriate.

### 4.3 Do Not Overload Components

Not every requirement belongs inside an existing component.

Keep responsibilities separate:
- Generic interaction behavior belongs in reusable UI components.
- Shared application services belong in shared services.
- Business rules belong in the appropriate domain or module.
- Module-specific workflows belong in the relevant module.
- Shell-level behavior belongs in the common shell.

Do not introduce business-specific logic into a generic component merely to avoid creating an appropriate supporting service.

### 4.4 Backward Compatibility

Shared component enhancements must preserve existing public APIs and behavior wherever practical.

Before changing a public API:
- Identify existing consumers.
- Assess compatibility.
- Prefer additive changes when suitable.
- Update affected consumers when necessary.
- Document breaking changes.
- Run regression tests.

### 4.5 Component Capability Completeness

Before approving a component enhancement, assess all relevant requirements, including:
- Normal operation.
- Loading state.
- Empty state.
- Validation.
- Error handling.
- Cancellation.
- Keyboard interaction.
- Accessibility.
- Responsive behavior.
- Asynchronous operations.
- Data preservation.
- Duplicate prevention.
- Permission handling.
- Existing consumer compatibility.

Not every component requires every capability. Implement only those appropriate to its responsibility.

## 5. USER WORKFLOW OPTIMIZATION POLICY

### 5.1 Minimum Necessary Effort

Design workflows to minimize unnecessary clicks, repeated selections, screen changes, and duplicate data entry.

Do not optimize click count at the expense of clarity, accessibility, data integrity, or error prevention.

### 5.2 Contextual Creation

When a user needs a related record that does not exist, provide a suitable creation option within the current workflow whenever practical.

Example: Machine Registration requires a Machine Model.

Expected workflow:
1. Enter the machine details.
2. Search for or select an existing model.
3. If the model does not exist, select Create Model.
4. Enter the required model details in an appropriate inline form or dialog.
5. Save the new model.
6. Automatically select the newly created model.
7. Continue the machine registration without losing existing input.
8. Save the machine.

Do not force the user to navigate to a separate model-management screen for a simple related-record creation task when contextual creation is appropriate.

### 5.3 Workflow Continuity

Opening, creating, editing, or cancelling a related record must not unexpectedly discard the current form state.

Preserve:
- Entered values.
- Existing selections.
- Relevant filters and search context.
- The user's current workflow position.

### 5.4 Automatic Association

After successful creation of a related record, automatically select or associate it with the current workflow when the relationship is unambiguous and authorized.

Do not require the user to search for and select the same newly created record again.

### 5.5 Appropriate Interaction Patterns

Select the interaction pattern according to task complexity.

Examples:
- Searchable selector for choosing an existing record.
- Inline creation for simple contextual additions.
- Compact dialog for small, focused tasks.
- Full-screen form for complex records.
- Inline editing for appropriate table operations.
- Bulk actions for operations involving multiple records.

Do not force every task into a dialog or put complex forms into unnecessarily small interfaces.

### 5.6 Smart Defaults

Use valid, contextually appropriate defaults when safe.

Never invent important business data or silently assume an ambiguous relationship.

### 5.7 Progressive Disclosure

Show essential information first.

Expose advanced fields, secondary actions, and optional configuration only when needed.

### 5.8 Clear Feedback

Provide understandable feedback for:
- Successful completion.
- Validation failures.
- Loading operations.
- Save failures.
- Duplicate records.
- Permission restrictions.
- Recoverable errors.

Error messages must explain the problem and, where practical, how to resolve it.

### 5.9 Confirmation Policy

Do not add unnecessary confirmation dialogs to routine, reversible actions.

Use confirmation when an action is destructive, irreversible, consequential, or requires explicit consent.

## 6. DESIGN SYSTEM ENFORCEMENT

All modules must follow the approved common design system.

The authoritative implementation must be identified by inspecting the existing repository.

Relevant existing implementation references include:
- Shared application CSS.
- UI utility functions.
- Icon definitions.
- Table implementation.
- Shared shell components.
- Module integration conventions.

The exact repository paths must be verified before modification.

### 6.1 Shared Visual Standards

Reuse approved:
- Colors.
- Typography.
- Spacing.
- Component dimensions.
- Icons.
- Buttons.
- Inputs.
- Dialogs.
- Tables.
- Navigation patterns.
- Responsive behavior.

Do not introduce arbitrary visual values or duplicate styles when approved shared definitions are available.

### 6.2 Table Standard

Use EC.table(...) for module lists and tabular data wherever the existing implementation supports the required behavior.

If the table lacks a required capability:
1. Identify the missing capability.
2. Inspect the existing table API.
3. Extend the shared table implementation when appropriate.
4. Preserve existing table consumers.
5. Test the new capability and existing behavior.
6. Reuse the enhanced implementation.

Do not introduce a separate table implementation merely to bypass a missing shared capability without documented architectural justification.

### 6.3 Module Independence

Modules must remain independent where required by the module architecture.

Shared UI behavior must be centralized without creating unnecessary dependencies between unrelated modules.

Do not make a module depend on another module's private implementation to obtain a shared capability.

## 7. COMPONENT API AND DOCUMENTATION

Every reusable component must have a clearly defined responsibility and documented public API appropriate to its complexity.

Document:
- Purpose.
- Supported use cases.
- Public API.
- Configuration options.
- Supported variants.
- Events and callbacks.
- Loading and error behavior.
- Validation behavior.
- Accessibility behavior.
- Limitations.
- Usage examples.
- Relevant tests.

Before adding a new component, inspect the existing library for an equivalent capability.

When a component is enhanced, update its documentation and examples.

Do not silently introduce undocumented behavior that changes existing consumers.

## 8. AUTOMATED VALIDATION AND TESTING

### 8.1 Component Tests

Test the new capability independently.

Verify relevant normal, boundary, and error conditions.

### 8.2 Integration Tests

Verify that the component works correctly within its intended screen or module.

### 8.3 Regression Tests

Verify that existing component behavior and affected modules continue to work.

### 8.4 UX Workflow Tests

Where practical, test the complete user journey.

For contextual record creation, verify:
- The creation action is available when appropriate.
- The new record is validated.
- Save success is handled correctly.
- The new record becomes available for selection.
- The newly created record is selected automatically when appropriate.
- Existing form input is preserved.
- Failures do not silently discard user data.
- Duplicate creation is handled appropriately.

### 8.5 Design Compliance

Check applicable requirements for:
- Approved component reuse.
- Design tokens.
- Icon conventions.
- Layout consistency.
- Keyboard interaction.
- Accessibility.
- Responsive behavior.

Use automated checks where practical and visual review where automation is insufficient.

### 8.6 Honest Test Reporting

Never claim that a test passed unless it was actually executed successfully.

If a test cannot be run, report the limitation explicitly.

## 9. CHANGE MANAGEMENT

Before changing a shared component, assess:
- The reason for the change.
- The missing capability being addressed.
- The component's existing consumers.
- The proposed API change.
- Compatibility implications.
- Potential regressions.
- Required documentation.
- Required tests.

Avoid unrelated refactoring within a narrowly scoped feature task unless it is necessary and justified.

Do not modify the common shell or global design system to solve a module-specific issue unless the change is appropriate to the shared responsibility.

## 10. EXCEPTION POLICY

A module-specific implementation is permitted when the requirement is genuinely specific to that module or when extending the existing component would violate its responsibility boundaries.

Before implementing an exception:
1. Explain why the existing component is insufficient.
2. Explain why extending it is inappropriate.
3. Identify the correct architectural layer.
4. Prefer a reusable companion component or shared service when appropriate.
5. Document the exception when it establishes a new pattern.

Do not use exceptions merely to avoid implementing a required shared capability.

## 11. AI CODING AGENT RULES

Before writing code:
- Inspect the relevant existing implementation.
- Read the applicable project instructions.
- Identify reusable components.
- Define the intended user workflow.
- Identify missing shared capabilities.
- Determine the correct architectural layer.

During implementation:
- Prefer approved components and shared services.
- Extend shared capabilities when appropriate.
- Avoid temporary UI workarounds.
- Preserve existing functionality.
- Avoid unrelated changes.
- Keep public APIs and module boundaries consistent.

Before completion:
- Run applicable tests.
- Verify the complete workflow.
- Check affected consumers.
- Update relevant documentation.
- Report unresolved limitations.

If requirements are ambiguous or a shared component change could have significant compatibility implications, explain the issue and propose a solution before making a risky change.

## 12. DEFINITION OF DONE

A feature is complete only when all applicable conditions are satisfied:

[ ] The user goal and workflow are clearly defined.
[ ] Unnecessary user actions have been minimized.
[ ] Existing reusable components have been inspected.
[ ] Missing capabilities have been identified.
[ ] Shared components have been enhanced when appropriate.
[ ] No unjustified module-specific workaround has been introduced.
[ ] Existing functionality remains compatible or approved migration is complete.
[ ] The module uses the appropriate shared capabilities.
[ ] Validation and error handling are implemented.
[ ] User input and workflow context are preserved where required.
[ ] Applicable component tests pass.
[ ] Applicable integration and regression tests pass.
[ ] Design and UX compliance have been checked.
[ ] Relevant documentation is updated.
[ ] Remaining limitations are reported accurately.

## 13. FINAL GOVERNING PRINCIPLE

OneApp OneMeet must not repeatedly solve the same problem in different modules.

When a legitimate shared requirement exposes a component limitation, improve the correct shared capability, validate the enhancement, and reuse it wherever appropriate.

When a workflow can be simplified safely, improve the workflow rather than forcing the user to adapt to unnecessary technical limitations.

Do not sacrifice architectural quality for short-term implementation speed.

The objective is a system in which:
- Shared components become more capable over time.
- Modules remain consistent and maintainable.
- UX workflows remain simple and efficient.
- Existing functionality remains reliable.
- New features require less duplicated code and less rework.

FINAL RULE:

IDENTIFY THE GAP.
IMPROVE THE RIGHT COMPONENT.
VERIFY COMPATIBILITY.
TEST THE WORKFLOW.
REUSE THE APPROVED CAPABILITY.

---

# PART II — UNIFIED APPLICATION ARCHITECTURE

## 14. ARCHITECTURE OBJECTIVE

OneApp OneMeet must support three user-facing environments while maintaining one authoritative data source:

1. **Desktop Application** — full application shell, administration, device integration, and heavy processing.
2. **LAN Server** — lightweight central API and database for shared business data.
3. **Phone / Other PC Browser** — responsive web access for approved lightweight workflows.

The goal is not to copy the entire Electron application onto a small server. The goal is to separate shared business data and lightweight web workflows from desktop-only capabilities.

### 14.1 Confirmed Initial Scope

The first LAN release is limited to:
- Machine Registry.
- Basic Invoice Entry.
- Stock / Inventory.
- Parts Requirement.

Heavy tasks remain desktop-only in the initial release:
- OCR and invoice image recognition.
- PDF conversion and intensive document processing.
- Heavy AI processing.
- Other functions that require desktop-native APIs, substantial CPU/RAM, or local hardware.

Do not expose a heavy-processing screen on the lightweight server merely because its UI can be rendered in a browser.

### 14.2 Initial Operating Constraints

- Network mode: LAN-only.
- External Internet access: not required for normal application use.
- Expected simultaneous users: 1–3.
- Client devices: desktop PC, phone browser, and optionally another PC browser.
- Server hardware: not yet finalized.
- Database: SQLite is the initial candidate, subject to concurrency, backup, and deployment tests.
- The server must be reachable by devices on the same local network.
- The application must not require a cloud subscription for this LAN workflow.

These are project requirements, not claims that the current repository already implements them.

## 15. TARGET SYSTEM DESIGN

```text
                         LOCAL LAN / WI-FI
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  ┌─────────────────────┐       ┌─────────────────────────────┐  │
│  │ Desktop PC           │       │ Mini PC / Raspberry Pi      │  │
│  │ OneApp Electron      │ HTTPS │ Headless LAN Server         │  │
│  │ Full shell + admin   ├──────►│ Node.js API                 │  │
│  │ OCR / PDF / AI       │       │ Authentication / RBAC       │  │
│  │ Desktop native APIs  │       │ Business modules / rules    │  │
│  │ Heavy job worker     │       │ Central SQLite database     │  │
│  └──────────┬──────────┘       └──────────────┬──────────────┘  │
│             │                                 ▲                 │
│             │                                 │ API             │
│             │                    ┌────────────┴─────────────┐  │
│             └───────────────────►│ Phone / Other PC Browser │  │
│                                  │ Responsive light UI      │  │
│                                  │ Data entry / inventory   │  │
│                                  └──────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

This diagram describes the target design. The actual process boundaries, ports, transport protocol, and module loading behavior must be verified in the repository before implementation.

### 15.1 Source-of-Truth Rule

The LAN server's central database is the authoritative source for the four shared business workflows.

- Desktop and phone must read/write shared records through the API.
- Browser clients must never open the server's SQLite file directly.
- Do not maintain separate independent copies of live shared business data on each client.
- Local caches are permitted only when their invalidation, conflict, and offline behavior are explicitly designed.
- OCR/PDF output produced by the desktop must be submitted to the API using a validated import/result workflow.
- Never assume that two separate SQLite files are synchronized merely because they contain similarly named tables.

### 15.2 Recommended Request Flow

1. Client opens the LAN application URL.
2. Server serves the lightweight web shell and permitted module assets.
3. Client authenticates when authentication is enabled/required by the configured deployment.
4. Client calls the API for permitted data.
5. Server validates the session, permissions, payload, and business rules.
6. Server performs the database transaction.
7. Server returns a structured success or error response.
8. Client updates the UI and preserves form state if the operation fails.

## 16. APPLICATION MODES AND RESPONSIBILITY BOUNDARIES

### 16.1 Desktop Mode

Desktop retains capabilities that require Electron, the host operating system, local hardware, or heavy processing.

Permitted responsibility examples:
- Full desktop shell.
- Local printer integration.
- Local file/folder access through approved bridge methods.
- OCR, PDF conversion, and heavy AI tasks.
- Heavy job worker.
- Administrative setup and maintenance tools where approved.
- Connecting to the LAN API for shared business data.

Desktop-only functionality must not be assumed available in a normal phone browser.

### 16.2 Headless Server Mode

The lightweight server must run without Electron, a visible desktop window, or a Chromium UI process.

Required responsibilities:
- Serve the approved lightweight web application.
- Expose the central business API.
- Validate requests and enforce authorization.
- Store shared business data.
- Provide health/status information.
- Log operational errors without exposing secrets.
- Support backup and restore procedures.
- Start and stop cleanly as a service.
- Bind to the intended LAN interface according to deployment configuration.

**Implementation constraint:** `src/main/host.js` is currently described as part of the Electron project. Do not assume it can run independently. Inspect its imports and dependencies. Extract reusable HTTP/server logic into a platform-neutral module where appropriate, and keep Electron-only dependencies separate.

### 16.3 Browser Mode

The browser interface must:
- Work on supported modern mobile and desktop browsers.
- Be responsive and touch-friendly.
- Expose only approved lightweight modules.
- Use the server API for data access.
- Never depend on `window.oneBrowser` for core browser workflows.
- Detect unsupported desktop-only actions and show a clear explanation rather than a broken control.
- Avoid downloading or loading heavy OCR/PDF/AI dependencies on the phone.

### 16.4 Shared Code Rule

Share domain logic, validation schemas, API contracts, and UI capabilities when they are genuinely platform-neutral.

Do not force desktop-native modules to run in the browser, and do not duplicate business rules in multiple clients when they belong on the server.

## 17. MODULE CAPABILITY MODEL

Each module must declare its runtime capabilities in its manifest or a documented equivalent. Verify the existing `module.json` schema before adding fields.

Suggested capability vocabulary:
- `browser`: can run in a normal browser.
- `serverApi`: requires the central server API.
- `desktopNative`: requires approved Electron/desktop bridge APIs.
- `heavyProcessing`: requires substantial local processing.
- `offlineRead`: may read an explicitly designed local cache.
- `adminOnly`: requires elevated application permission.

Example conceptual declaration:

```json
{
  "id": "machine-registry",
  "title": "Machine Registry",
  "capabilities": ["browser", "serverApi"]
}
```

This is a proposed schema example, not a claim that the current loader accepts this field.

### 17.1 Initial Module Classification

| Module | Browser | Server API | Desktop Native / Heavy |
|---|---:|---:|---:|
| Machine Registry | Yes | Yes | No |
| Basic Invoice Entry | Yes | Yes | No |
| Stock / Inventory | Yes | Yes | No |
| Parts Requirement | Yes | Yes | No |
| OCR Processing | No for initial release | Result submission only, if required | Yes |
| PDF Conversion | No for initial release | Result submission only, if required | Yes |
| Heavy AI Processing | No for initial release | Job/result API only, if required | Yes |

The exact scope of each module must be approved before implementation. A module may have a lightweight browser entry screen while its heavy processing action remains desktop-only, but the boundary must be explicit.

## 18. API AND DATA DESIGN RULES

### 18.1 API Contract

- Use a documented, versioned API contract.
- Keep request and response formats consistent.
- Validate all input on the server; client validation is supplementary.
- Use stable record IDs rather than display names as relationships.
- Return actionable, structured errors.
- Apply pagination or bounded result sizes to potentially large lists.
- Avoid exposing internal filesystem paths, stack traces, secrets, or raw database errors.
- Define duplicate detection and unique constraints for important business identifiers.
- Use transactions for multi-step data changes.
- Keep business rules in the server/domain layer when they must apply equally to all clients.

### 18.2 Initial Domain Areas

The first release must define and document data ownership for:
- Machine Registry and Machine Models.
- Basic Invoice Entry.
- Stock / Inventory.
- Parts Requirement.

Before implementation, inspect the current schema and determine which entities already exist. Do not create duplicate tables or parallel models simply because a new UI screen is being added.

### 18.3 Related-Record Creation

Use the existing UX constitution's contextual creation rule. For example, when registering a machine:
1. Search/select an existing model.
2. If missing, create the model in a compact inline form/dialog.
3. Validate and save through the API.
4. Return the new record ID and display data.
5. Automatically select it in the machine form.
6. Preserve all other entered values.
7. Continue the machine save workflow.

The server must enforce permissions and uniqueness even if the UI offers a convenient inline flow.

### 18.4 SQLite Starting Policy

SQLite is a reasonable initial candidate for a small 1–3-user LAN deployment, but it must be validated under realistic use.

- Only the server process accesses the live database file.
- Use parameterized queries or a safe data-access layer.
- Use transactions for related writes.
- Set and test appropriate busy-timeout/concurrency handling.
- Keep the database on local storage, not a shared network folder.
- Define backup frequency, retention, restore steps, and a recovery test.
- Do not copy a live database file as a substitute for a consistent backup.
- If concurrent writes, data size, reliability, or remote administration exceed SQLite's practical limits, reassess the database before adding complexity elsewhere.

## 19. DESKTOP HEAVY-PROCESSING WORKFLOW

The initial implementation does not need a distributed job system unless the code and workflow require it. Keep the first version simple.

If a phone/browser must submit a document that needs desktop OCR or PDF processing, use this optional workflow:

1. Browser submits an authorized job request or source file to the server.
2. Server validates the upload and records a job with a stable ID and status.
3. Desktop worker checks for available jobs while connected.
4. Desktop downloads/claims a job using an authenticated API.
5. Desktop performs OCR/PDF/AI processing locally.
6. Desktop submits structured results and any approved output files.
7. Server validates and stores the result.
8. Browser refreshes or polls the job status and presents the result for user review.

Possible job states:
- `queued`
- `claimed`
- `processing`
- `needs_review`
- `completed`
- `failed`
- `cancelled`

Use these states only if the job workflow is implemented. Add claim expiry, retry limits, duplicate submission protection, and failure recovery before relying on the queue operationally.

If the desktop is offline, queued jobs must remain safely pending. Do not imply that OCR or heavy processing will occur on the Mini Server.

## 20. LAN NETWORK AND SECURITY POLICY

### 20.1 Initial Network Decision

The first release is **LAN-only**:
- No public Internet exposure.
- No router port forwarding.
- No public IP hosting.
- No requirement for external mobile-data access.
- Phone access is available when the phone joins the same local Wi-Fi/LAN.
- If the LAN has no Internet connection, local operations should continue provided the router/server and required services remain powered.

If remote access is required later, treat it as a separately approved architecture change; do not simply expose the current port to the Internet.

### 20.2 Binding and Firewall

- Configure the server to listen on the intended LAN interface, not every interface by default without review.
- Restrict the server port through the host firewall to the intended private network.
- Do not assume that a private IP address alone provides authentication or security.
- Use a stable DHCP reservation or documented local hostname where practical.
- Detect port conflicts and show the actual listening address and port.
- Avoid silently selecting a different port if clients depend on a fixed URL; if dynamic fallback is retained, publish the selected port through a clear discovery/status mechanism.

### 20.3 Transport Security

- Use HTTPS on LAN where feasible, with a certificate strategy that is practical for the supported devices.
- Do not instruct users to ignore certificate warnings as a normal operating procedure.
- If the initial deployment uses HTTP on a trusted isolated LAN, document the risk and avoid transmitting reusable secrets in clear text. Treat this as a deployment trade-off, not a universal secure default.
- Validate `Host` and `Origin` where applicable, and implement CSRF protections for cookie-based sessions.
- Do not treat CORS as authentication.

### 20.4 Authentication and Authorization

- Define user roles and permissions before exposing write operations.
- Apply authorization on the server for every protected operation.
- A PIN can be an additional access control but must not be treated as a complete security design by itself.
- Store password/PIN verifiers securely using an appropriate password-hashing mechanism; never store plaintext credentials.
- Rate-limit repeated authentication failures.
- Use session expiry and revocation rules.
- Protect administrative actions separately.
- Log security-relevant events without logging passwords, tokens, or sensitive payloads.

### 20.5 API and File Security

- Validate request body size, types, IDs, and allowed values.
- Prevent path traversal and arbitrary filesystem access.
- Use an explicit allowlist for file operations.
- Keep Electron `contextIsolation` and sandbox protections enabled where compatible with the existing app.
- Expose only the minimum required preload bridge methods.
- Validate calls in the main process; renderer-side checks alone are insufficient.
- Keep secrets out of module manifests and browser JavaScript.
- Do not allow an untrusted module to call arbitrary OS commands or access arbitrary local paths.

## 21. DEPLOYMENT AND HARDWARE STRATEGY

### 21.1 Do Not Buy Hardware Before a Baseline Test

First run a headless API prototype on the existing Windows desktop, then measure:
- Idle RAM usage.
- RAM with 1–3 active clients.
- CPU during typical CRUD operations.
- Database size and write behavior.
- Startup and restart reliability.
- Network reachability from phone and another PC.

Only then choose the Mini PC or single-board computer. The estimates below are starting guidance, not benchmark guarantees.

### 21.2 Initial Hardware Guidance

- **2 GB RAM:** possible starting point for a tightly scoped Node.js API and small database, but leaves limited headroom.
- **4 GB RAM:** safer general starting point and more room for maintenance tools and future modules.
- **Storage:** use reliable local storage with adequate free space; 32–64 GB can be a starting range for a minimal system, depending on logs, backups, uploads, and growth.
- **Network:** Ethernet is preferred for the server when available; Wi-Fi can be used if the network is reliable.
- **OS:** a supported 64-bit lightweight Linux distribution is a candidate for a dedicated headless server.
- **ARM compatibility:** verify Node.js version, native npm dependencies, architecture support, and installation steps before selecting Raspberry Pi/Orange Pi hardware.
- **x86 mini PC:** may offer simpler compatibility with desktop-oriented Node dependencies and common storage options.

### 21.3 Server Lifecycle

Document:
- Install/setup steps.
- Configuration location.
- LAN address and port discovery.
- Service start/stop/restart.
- Startup on boot.
- Log access and log rotation.
- Backup and restore.
- Database migration.
- Upgrade and rollback.
- Recovery after power loss.

Do not mark the server production-ready until backup restoration and restart recovery have been tested.

## 22. DATA INTEGRITY, SYNC, AND FAILURE BEHAVIOR

### 22.1 Central Online Mode

In the first release, shared workflows use the central server as the source of truth. A client must show a clear connection/error state when the server is unavailable.

### 22.2 Offline Behavior

Do not promise full offline writes or conflict-free synchronization unless implemented and tested.

If offline entry is later required:
- Define local queue storage and retry behavior.
- Use stable IDs and idempotency keys.
- Define conflict detection and resolution.
- Prevent duplicate invoice, stock, or requirement submissions.
- Show pending/synced/failed states.
- Never silently overwrite a newer server record with stale local data.

### 22.3 Auditability

For important inventory and invoice operations, assess whether the system needs:
- Created/updated timestamps.
- User attribution.
- Change history.
- Stock movement records rather than direct quantity edits alone.
- Reversal/correction workflows.
- Duplicate prevention.

These requirements must be confirmed against the actual business process and current schema before implementation.

## 23. RESPONSIVE UI RULES FOR PHONE AND DESKTOP

- Design phone workflows for touch, narrow screens, and minimal typing.
- Use appropriate input types and numeric keyboards for quantities and amounts.
- Keep primary actions visible without requiring unnecessary navigation.
- Do not place dense desktop-only tables unchanged on narrow screens.
- Prefer compact list/detail patterns or responsive tables for phone.
- Preserve unsaved values on validation or network errors.
- Disable duplicate submissions while a save is in progress.
- Show save success/failure clearly.
- Use consistent icon sizing and visual hierarchy throughout the shell and modules.
- Keep icons recognizable, standard-sized, and aligned with the existing design system.
- Do not use an icon as the only label for an unfamiliar or consequential action; provide text or accessible labeling.

## 24. IMPLEMENTATION PHASES

### Phase 0 — Repository Audit
- Inspect the actual repository and current branch.
- Verify package scripts, Electron entry points, `host.js`, module loader, preload bridge, UI SDK, and database/API code.
- Identify which code is reusable outside Electron.
- Map current modules, data stores, ports, and security boundaries.
- Produce a gap report before making structural changes.

### Phase 1 — Headless Server Proof of Concept
- Extract or create a platform-neutral HTTP/API server.
- Run it without Electron.
- Add health/status endpoint and configuration.
- Confirm LAN access from a phone and another PC.
- Verify firewall behavior and error handling.

### Phase 2 — Central Data Layer
- Inspect existing schemas before creating migrations.
- Establish API contracts and server-side validation.
- Add authentication/authorization suitable for LAN deployment.
- Test concurrent use by 1–3 clients.
- Add backup/restore and migration procedures.

### Phase 3 — Four Lightweight Modules
Implement in this order unless the repository audit identifies a dependency:
1. Machine Registry.
2. Stock / Inventory.
3. Parts Requirement.
4. Basic Invoice Entry.

Reuse the shared design system and components. The exact order can be changed when domain dependencies justify it.

### Phase 4 — Desktop Integration
- Connect desktop modules to the central API for shared records.
- Keep desktop-native APIs behind the existing approved bridge.
- Separate heavy processing from lightweight server request handling.
- Ensure browser code never invokes Electron-only APIs.

### Phase 5 — Optional Heavy Job Queue
Only implement if a phone-to-desktop processing workflow is required.
- Define job contracts, state transitions, retries, idempotency, file validation, and review UX.
- Test desktop offline/online transitions.

### Phase 6 — Deployment and Acceptance
- Test on target Mini PC/board.
- Test phone and desktop access on LAN.
- Test no-Internet operation.
- Test restart, backup restore, permissions, and common failure cases.
- Measure resource usage and revise hardware assumptions.
- Publish setup and maintenance documentation.

## 25. ACCEPTANCE CHECKLIST FOR THE LAN RELEASE

The initial release is not complete until all applicable checks pass:

[ ] Server starts without launching Electron or a Chromium window.
[ ] Server is reachable from approved devices on the same LAN.
[ ] Public Internet access and router port forwarding are not required.
[ ] Machine Registry works through the server API.
[ ] Basic Invoice Entry works through the server API.
[ ] Stock / Inventory works through the server API.
[ ] Parts Requirement works through the server API.
[ ] Phone UI is usable at a narrow screen width.
[ ] Desktop and phone see the same saved shared records.
[ ] No client directly accesses the live server database file.
[ ] Server-side validation and authorization are enforced.
[ ] Duplicate submissions and relevant uniqueness constraints are handled.
[ ] Network/save errors do not silently discard user input.
[ ] Electron-only features are not exposed as functional browser actions.
[ ] OCR/PDF/heavy AI remain desktop-only unless a documented job workflow is added.
[ ] Backup and restore have been tested.
[ ] Restart and power-loss recovery have been tested to an acceptable level.
[ ] Logs do not expose credentials or tokens.
[ ] Shared component enhancements have component and regression tests.
[ ] Existing module behavior has been checked for regressions.
[ ] Actual tests and known limitations are reported honestly.

## 26. REQUIRED AI AGENT REPORT FORMAT

For each substantial architecture or shared-component change, report:

1. **Goal:** user workflow being improved.
2. **Repository evidence:** files and APIs actually inspected.
3. **Current behavior:** verified facts, not assumptions.
4. **Capability gap:** what is missing.
5. **Responsibility boundary:** UI component, shared service, module, API, or desktop bridge.
6. **Implementation:** files and behavior changed.
7. **Compatibility:** affected consumers and migration needs.
8. **Security/data impact:** permissions, validation, data integrity, and risks.
9. **Tests:** exact commands/tests run and actual results.
10. **Remaining work:** unimplemented requirements and known limitations.

Never report proposed architecture as already implemented. Never claim a file was changed, a server was tested, or a test passed unless that action actually occurred.

## 27. ARCHITECTURE DECISIONS REGISTER

| Decision | Initial Direction | Status |
|---|---|---|
| Network reach | LAN-only | Selected |
| Simultaneous users | 1–3 | Selected |
| Lightweight server screens | Machine Registry, Basic Invoice Entry, Stock / Inventory, Parts Requirement | Selected |
| Heavy processing | Desktop | Selected |
| Phone access | Browser UI | Selected |
| Shared business data | Central API and server-owned database | Target architecture |
| Initial database | SQLite candidate | Validate with prototype |
| Server hardware | Mini PC / Raspberry Pi / Orange Pi not finalized | Open |
| HTTP vs HTTPS for local LAN | Choose after certificate/deployment review | Open |
| Authentication roles and PIN policy | Define during repository/security audit | Open |
| Offline writes | Not promised in initial release | Deferred |
| Desktop processing queue | Optional; implement only if needed | Deferred |
| Exact API routes and database schema | Must be derived from source-code audit | Open |

## 28. FINAL ARCHITECTURE PRINCIPLE

OneApp OneMeet is one product with platform-appropriate execution environments, not three independent applications with competing data stores.

- The server owns shared business data and server-enforced rules.
- The desktop owns approved desktop-native capabilities and heavy processing.
- The phone/browser provides simple, responsive access to permitted workflows.
- Shared components evolve through a documented capability-gap process.
- Modules use approved shared capabilities rather than creating avoidable workarounds.
- Network exposure, authentication, data integrity, and recovery are designed explicitly.
- Architecture proposals remain proposals until implementation and tests prove them.

FINAL RULE:

DESIGN THE WORKFLOW.
INSPECT THE REPOSITORY.
DEFINE RESPONSIBILITY BOUNDARIES.
BUILD THE SHARED CAPABILITY.
KEEP ONE AUTHORITATIVE DATA SOURCE.
SECURE THE LAN API.
TEST THE COMPLETE USER JOURNEY.
DOCUMENT WHAT IS VERIFIED — AND WHAT IS NOT.

