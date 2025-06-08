# Compliance PLM Application

The `compliance_plm` application orchestrates several components, primarily `create_research_report` and `dev_planner`, as defined in `compliance_plm_manifest.json`.

## Component Handoff and Resumability

The orchestrated components (`create_research_report`, `dev_planner`) are designed to support enhanced handoff and resumability features. This functionality relies on each component instance receiving a unique `COMPONENT_INSTANCE_ID` from the invoking system (typically the main Aidentity/AI). This ID allows them to manage their own state and handoff notes (e.g., `[COMPONENT_INSTANCE_ID]_handoff.md`).

**Current Status:**

As of the last review, the main Aidentity/AI framework has not yet implemented the feature to dynamically generate and inject `COMPONENT_INSTANCE_ID`s into components invoked by PromptApps like `compliance_plm`.

**Impact on `compliance_plm`:**

Because `compliance_plm` relies on the Aidentity to provide these `COMPONENT_INSTANCE_ID`s to its sub-components, their full designed handoff and resumability features may not be active or may operate in a limited capacity when these components are run through `compliance_plm`.

This functionality is expected to become fully available once the Aidentity framework is updated to support the new hierarchical handoff protocol, which includes providing `COMPONENT_INSTANCE_ID`s to all component instances. No changes to `compliance_plm_manifest.json` are required for this future enhancement, as the manifest structure is already compatible with such injections.
