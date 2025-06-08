---
id: Trait_Core_AIdentity_UpdateHandoffNotes
name: "Update Handoff Notes at Session End"
category: "aidentity_lifecycle"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.F"
description: "Prescribes the procedure for archiving existing handoff notes and writing a new session summary (detailed or concise based on Framework_Full_Handoff_Notes_Enabled) to handoff_notes.md at session end."
strictness_default: "Rule"
version: "1.0"
keywords: ["handoff notes", "session summary", "documentation", "persistence", "aidentity", "continuity"]
related_traits: ["Trait_Core_Guideline_CommittingHandoffNotesAtSessionConclusion"]
---
**Full Directive Text:**
F. **Update Handoff Notes (`promptu_dev/aidentity/handoff_notes.md` or `promptu/aidentity/handoff_notes.md`):**
    0.  Archive Current Handoff Notes Before Appending New Summary:
        a.  Assume you have prepared your session summary, but it has not yet been written to the active handoff notes file.
        b.  Define paths based on `Is_Developer_Session` (from I.A):
           IF `Is_Developer_Session` is true:
               `current_handoff_notes_path = "promptu_dev/aidentity/handoff_notes.md"`
               `current_handoff_archive_dir = "promptu_dev/aidentity/archive/"`
               `current_handoff_notes_archive_subdir = current_handoff_archive_dir + "handoff_notes/"`
               INFO "Using Developer handoff paths for session summary archival."
           ELSE (User Mode):
               `current_handoff_notes_path = "promptu/aidentity/handoff_notes.md"`
               `current_handoff_archive_dir = "promptu/aidentity/archive/"`
               `current_handoff_notes_archive_subdir = current_handoff_archive_dir + "handoff_notes/"`
               INFO "Using User handoff paths for session summary archival."
        c.  Check if `current_handoff_notes_path` exists and contains any content.
        d.  If yes:
            i.  Read its content (`current_notes_content_for_archive`).
            ii. Generate a `current_session_date_time_marker_for_archive` (e.g., YYYYMMDD_HHMMSS).
            iii. Ensure `current_handoff_notes_archive_subdir` exists in the workspace. If `current_handoff_archive_dir` or `current_handoff_notes_archive_subdir` do not exist, create them.
            iv. Define `archive_file_path_for_current = current_handoff_notes_archive_subdir + "handoff_notes_" + current_session_date_time_marker_for_archive + ".md"`.
            v.  Create `archive_file_path_for_current` and write `current_notes_content_for_archive` into it.
            vi. Empty `current_handoff_notes_path` in the workspace (e.g., by writing an empty string to it or truncating it). This ensures it's clean before the new summary is appended.
    1.  **Review `Framework_Full_Handoff_Notes_Enabled` (determined in Section K.4).**

    2.  **Construct Session Summary Content:**
        a.  Initialize `SessionSummaryContent` string.
        b.  **Append Active and Recent Tasks Section:**
            i.  Append `### Active and Recent Tasks\n` to `SessionSummaryContent`.
            ii. Append `- Main AI Task: [session.current_ai_task_description]\n` (or equivalent variable).
            iii. If `session.AppInstanceID` is set and not empty:
                1.  Invoke `[[USE_TRAIT Trait_Core_ManageAppHandoffNote.get_app_handoff_details]]` with inputs:
                    *   `appInstanceID: session.AppInstanceID`
                    *   `appName: session.AppName`
                2.  Store the result as `app_details`.
                3.  If `app_details` is successfully retrieved:
                    Append to `SessionSummaryContent`:
                    ```markdown
                    - PromptApp: [session.AppName] (ID: [session.AppInstanceID])
                      - Status: [app_details.status]
                      - Handoff Note: ./archive/apps/[session.AppInstanceID]_[session.AppName]_handoff.md
                    ```
                    If `app_details.components` exists and is not empty:
                        Append `  - Components:\n` to `SessionSummaryContent`.
                        For each `component` in `app_details.components`:
                            Append to `SessionSummaryContent`:
                            ```markdown
                              - Name: [component.componentName] (ID: [component.componentInstanceID])
                                - Status: [component.status]
                                - Handoff Note: ./archive/components/[component.componentInstanceID]_handoff.md
                            ```
                4.  Else (if `app_details` could not be retrieved):
                    Append to `SessionSummaryContent`:
                    ```markdown
                    - PromptApp: [session.AppName] (ID: [session.AppInstanceID]) - Status: Error retrieving details.
                    ```
            iv. Append `\n---\n` (horizontal rule) to `SessionSummaryContent` after the Active/Recent Tasks section.

        c.  **Append Standard Summary Details based on `Framework_Full_Handoff_Notes_Enabled`:**
            IF `Framework_Full_Handoff_Notes_Enabled` is `true`:
                Append to `SessionSummaryContent`:
                *   User feedback/requests you addressed.
                *   A detailed account of changes made to this framework's documents or other deliverables (e.g., `core_planning_instructions.txt`, `available_add_ons_manifest.md`, `compliance_plm_manifest.json`).
                *   The state in which deliverables were left (e.g., specific commits, branches).
                *   Any pending items or suggestions for the next Jules instance.
            ELSE (if `Framework_Full_Handoff_Notes_Enabled` is `false`):
                Append to `SessionSummaryContent`:
                *   Major architectural changes implemented.
                *   Key decisions made that affect the framework's evolution.
                *   The final state of key deliverables (e.g., new components created, significant refactoring of core files like `core_planning_instructions.txt`).
                *   (Avoid detailed chronological accounts; aim for a high-level recap of critical developments).

    3.  **Write Session Summary to Handoff File:**
        a.  Write the fully constructed `SessionSummaryContent` to the now-empty `current_handoff_notes_path`.
        b.  IMPORTANT: Ensure the 'Framework Performance Feedback Log' section within `handoff_notes.md` is NOT included or appended to in this file. Your new summary block should be appended after existing session summaries but before any 'Framework Performance Feedback Log' section if one accidentally exists. The goal is to phase out direct updates to that specific log section from this file.
    4.  **General Note on Handoff Notes Archive:**
        a.  Be aware that a more detailed historical archive of handoff notes might exist at `promptu_dev/dev/handoff_notes_full_archive_20250602.md`.
        b.  You should NOT read this archive file unless you determine it is critical for understanding specific historical context to fulfill the current user request AND you explicitly ask for and receive user permission to do so. Your primary source for ongoing handoff information is the main `handoff_notes.md` file.

**Implementation Notes:**
- This is a mandatory procedure at session end.
- Determine `Is_Developer_Session` to use correct paths.
- **Archival (Step 0):** Before writing the new summary, any existing content in `handoff_notes.md` MUST be read, archived to `.../archive/handoff_notes/handoff_notes_YYYYMMDD_HHMMSS.md`, and then `handoff_notes.md` must be emptied.
- **New Summary Content (Steps 1-3):**
    - Construct the "Active and Recent Tasks" section.
    - Append detailed or concise summary based on `Framework_Full_Handoff_Notes_Enabled`.
    - Write the combined content to the handoff file.
- Avoid including/updating the 'Framework Performance Feedback Log' section.
- Note restrictions on accessing `promptu_dev/dev/handoff_notes_full_archive_20250602.md` (Step 4).
EOL
