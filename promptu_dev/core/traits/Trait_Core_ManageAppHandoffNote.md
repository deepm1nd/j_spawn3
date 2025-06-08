# Trait: Manage App Handoff Note

**Purpose:** Manages the creation, updating, and reading of app-specific handoff notes.

**Handoff Note Path:** `promptu_dev/aidentity/archive/apps/[session.AppInstanceID]_[AppName]_handoff.md` (Note: `AppName` should be retrieved from session or manifest).

**Core Functions:**

*   **`create_app_handoff_note(appName, appInstanceID)`:**
    1.  If the handoff note does not exist at the path, create it.
    2.  Initial content:
        ```markdown
        # App Handoff: [appName]

        **AppInstanceID:** `[appInstanceID]`
        **Status:** Initializing
        **Timestamp:** `[Current Timestamp]`

        ## Invoked Components:
        | ComponentInstanceID | ComponentName | Status    | Invoked At | Handoff Note Link |
        |---------------------|---------------|-----------|------------|-------------------|
        ```

*   **`update_app_status(appInstanceID, appName, newStatus)`:**
    1.  Read the app handoff note.
    2.  Update the `Status:` field.
    3.  Write changes back to the file.

*   **`record_component_invocation(appInstanceID, appName, componentInstanceID, componentName, componentStatus, invocationTimestamp)`:**
    1.  Read the app handoff note.
    2.  Append a new row to the "Invoked Components" table:
        `| [componentInstanceID] | [componentName] | [componentStatus] | [invocationTimestamp] | ../components/[componentInstanceID]_handoff.md |`
    3.  Write changes back to the file.

*   **`update_component_status_in_app_handoff(appInstanceID, appName, componentInstanceID, newStatus)`:**
    1.  Read the app handoff note.
    2.  Find the row matching `componentInstanceID` in the "Invoked Components" table.
    3.  Update the `Status` cell for that row.
    4.  Write changes back to the file.

*   **`get_app_handoff_details(appInstanceID, appName)`:**
    1.  Read the app handoff note.
    2.  Parse and return its structured content (status, list of components with their details). Returns null or error if not found.

**Integration:**
*   `create_app_handoff_note` called during Aidentity initialization for a PromptApp (after `AppInstanceID` generation).
*   `record_component_invocation` and `update_component_status_in_app_handoff` called by `Trait_Core_ExecutePromptAppPath.md`.
*   `update_app_status` called by Aidentity's main operational loop.
*   `get_app_handoff_details` used by `Trait_Core_AIdentity_UpdateHandoffNotes.md`.
