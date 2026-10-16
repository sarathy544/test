# test
# OneApp OneMeet — Component Evolution & UX Workflow Constitution

Version: 1.0.0
Status: Proposed Standard
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
