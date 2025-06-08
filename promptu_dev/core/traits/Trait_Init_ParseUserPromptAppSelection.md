---
id: Trait_Init_ParseUserPromptAppSelection
name: "Parse User PromptApp Selection and Configuration"
category: "initialization_config"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "I.J"
description: "Parses [[USER_APP_SELECTION]] from post_promptu.txt, identifies selected PromptApp and its tasks from [[USER_[Selected_PromptAppName]_CONFIGURATION]], and handles warnings."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "promptapp", "user selection", "configuration", "parsing"]
related_traits: ["Trait_Init_DiscoverLoadPromptAppManifests"]
---
**Full Directive Text:**
J. **Parse User PromptApp Selection & Configuration:**
    1.  Initialize `Selected_PromptAppName`: null.
    2.  Initialize `Selected_App_Tasks_Ordered`: empty list.
    3.  Scan `post_promptu.txt` for the `[[USER_APP_SELECTION]]` block.
    4.  Identify lines beginning with `[x]` to extract the selected `potential_app_name`.
    5.  If multiple promptApps are selected, issue a warning: "Warning: Multiple promptApps selected. Using the first one: '[first_selected_app_name]'." Set `Selected_PromptAppName` to the first valid one found in `Available_PromptApps_Manifest_Map`.
    6.  If only one `[x]` is found, set `Selected_PromptAppName` to that `potential_app_name`, provided it exists as a key in `Available_PromptApps_Manifest_Map`. If not found in the map, issue a warning and set `Selected_PromptAppName` to null.
    7.  If `Selected_PromptAppName` is set and valid:
        a.  Define delimiters for its configuration block: `start_config_delim = "[[USER_" + Selected_PromptAppName + "_CONFIGURATION]]"`, `end_config_delim = "[[END_USER_" + Selected_PromptAppName + "_CONFIGURATION]]"`.
        b.  Search the *entire original user-provided main prompt file content* (typically `post_promptu.txt`) for text between these delimiters.
        c.  If found:
            i.  Parse the content line by line.
            ii. Identify lines starting with `[x]` (case-insensitive for 'x') followed by a task name (e.g., `[x] My Task Name`). Extract "My Task Name".
            iii. Add each such extracted task name to `Selected_App_Tasks_Ordered`, maintaining the order from the block.
            iv. Store the raw string content of this block in `App_Specific_Configs_Content_Map` with `Selected_PromptAppName` as the key (for reference or audit).
        d.  If the configuration block is missing: Issue a warning: "Warning: PromptApp '[Selected_PromptAppName]' is selected, but its configuration block '[[USER_[Selected_PromptAppName]_CONFIGURATION]]' was not found. No tasks can be run." Set `Selected_PromptAppName` to null.
        e.  If the block is found but no tasks are checked `[x]`: Issue a warning: "Warning: PromptApp '[Selected_PromptAppName]' configuration block found, but no tasks were selected with '[x]'. No tasks will be run." Set `Selected_PromptAppName` to null.

**Implementation Notes:**
- Parses `[[USER_APP_SELECTION]]` from `post_promptu.txt` to find the chosen `Selected_PromptAppName`.
- Handles cases of multiple selections or invalid selections against `Available_PromptApps_Manifest_Map`.
- Parses `[[USER_[Selected_PromptAppName]_CONFIGURATION]]` from the main prompt for selected tasks (`[x] Task Name`).
- Populates `Selected_App_Tasks_Ordered` and `App_Specific_Configs_Content_Map`.
- Issues warnings for missing config blocks or no selected tasks.
EOL
