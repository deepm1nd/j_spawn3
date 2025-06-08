---
id: Trait_Init_ParseUserFrameworkSettings
name: "Parse User Framework Settings (Handoff Notes Enabled)"
category: "initialization_config"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "I.K"
description: "Parses the [[USER_FRAMEWORK_SETTINGS]] block from post_promptu.txt to determine if Framework_Full_Handoff_Notes_Enabled is true or false."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "framework settings", "handoff notes", "configuration", "parsing"]
related_traits: ["Trait_Core_AIdentity_UpdateHandoffNotes"]
---
**Full Directive Text:**
K. **Parse User Framework Settings:**
    1.  Initialize `Framework_Full_Handoff_Notes_Enabled`: `false`.
    2.  Define delimiters for the framework settings block: `start_fs_delim = "[[USER_FRAMEWORK_SETTINGS]]"`, `end_fs_delim = "[[END_USER_FRAMEWORK_SETTINGS]]"`.
    3.  Search the *entire original user-provided main prompt file content* (typically the user's configured copy of `post_promptu.txt`) for text between these delimiters.
    4.  If the `[[USER_FRAMEWORK_SETTINGS]]` block is found:
        a.  Within the extracted block content, search for a line that looks like a checkbox for "Full_Handoff_Notes_Logging_Enabled". This typically means a line starting with `[x]` (case-insensitive for 'x') or `[ ]`, followed by the text `Full_Handoff_Notes_Logging_Enabled`.
        b.  If such a line is found and it starts with `[x]` (indicating checked):
            Set `Framework_Full_Handoff_Notes_Enabled = true`.
            INFO "Full_Handoff_Notes_Logging_Enabled is TRUE."
        c.  Else (if found but not `[x]`, or if the specific checkbox line is not found within the block):
            `Framework_Full_Handoff_Notes_Enabled` remains `false`.
            INFO "Full_Handoff_Notes_Logging_Enabled is FALSE (default or explicitly unchecked)."
    5.  Else (if the `[[USER_FRAMEWORK_SETTINGS]]` block itself is not found):
        `Framework_Full_Handoff_Notes_Enabled` remains `false`.
        INFO "[[USER_FRAMEWORK_SETTINGS]] block not found. Full_Handoff_Notes_Logging_Enabled is FALSE (default)."

**Implementation Notes:**
- Parses `[[USER_FRAMEWORK_SETTINGS]]` from `post_promptu.txt`.
- Specifically looks for `[x] Full_Handoff_Notes_Logging_Enabled` to set `Framework_Full_Handoff_Notes_Enabled`.
- Defaults to `false` if block or checkbox is not found or not checked.
- This flag influences behavior in Trait_Core_AIdentity_UpdateHandoffNotes (IV.F).
EOL
