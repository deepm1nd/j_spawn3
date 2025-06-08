---
id: Trait_Core_Output_FinalCommitMessageByPlanningAI
name: "Final Commit Message by Planning AI Format"
category: "output_formatting"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.E"
description: "Specifies the structure and content for the AI's final commit message, based on whether a PromptApp or Add-on was processed."
strictness_default: "Guideline"
version: "1.0"
keywords: ["commit message", "git", "output format", "documentation", "iep"]
related_traits: ["Trait_Core_Guideline_BranchNamingConventionsForCommits"]
---
**Full Directive Text:**
E. **Final Commit Message by Planning AI:**
    Adhere to IEP (from `promptu/core/base_iep.txt`).
    *   **IF `Selected_PromptAppName` was processed for a task:**
        *   Task-ID: `PROCESSED_PROMPT_APP_[Selected_PromptAppName]_TASK_[current_task_user_name]` (use the user-friendly task name).
        *   Notes:
            *   `Selected_PromptAppName`: [Selected_PromptAppName]
            *   `Task Processed`: [current_task_user_name]
            *   Component Used: [component_name_to_load]
            *   Warnings for multiple promptApp selections, missing/invalid manifests, missing promptApp config block, or no tasks checked.
            *   Note if the task was iterable and user was prompted to continue/halt for next task.
            *   Self-checks, confidence, etc.
    *   **ELSE (Add-on or core utility processing):**
        *   Task-ID: e.g., `PROCESSED_PRIMARY_ADDON_[Active_Primary_Addon_AppName]` or `CORE_UTILITY_PROCESSING_ONLY`.
        *   Notes:
            *   `Active_Primary_Addon_AppName` processed (if applicable).
            *   Warnings for multiple primary add-on selections, missing add-on/util files.
            *   Summary of other add-ons processed from `Inheritable_Addons_Content_Ordered_List`.
            *   Note if any selected add-on expected an app-specific config block but it was not found.
            *   Self-checks, confidence, etc.

**Implementation Notes:**
- The AI needs to determine if a PromptApp task or an Add-on/utility was the primary focus to select the correct Task-ID format.
- Variables like `Selected_PromptAppName`, `current_task_user_name`, `component_name_to_load`, `Active_Primary_Addon_AppName` must be tracked during the session.
- The "Notes" section should be populated with relevant warnings and a summary of activities/checks.
- Reference to `promptu/core/base_iep.txt` implies that the overall structure of the commit message should follow the Information Exchange Protocol.
EOL
