---
id: Trait_Init_DiscoverLoadAddons
name: "Discover and Load Add-on Components"
category: "initialization_config"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "I.F"
description: "A multi-step process to parse user add-on selections, load their content, identify primary add-ons, and populate inheritable add-ons list. Includes handling app-specific configs from the main prompt."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "add-ons", "component loading", "configuration", "parsing"]
related_traits: []
---
**Full Directive Text:**
F. **Discover and Load Add-on Components:**
    1.  **Parse User Add-on Selections:**
        a.  Scan the `[[USER_ADDON_SELECTION]]` block from `post_promptu.txt`.
        b.  Identify lines beginning with `[x]` to extract the selected `addon_app_name`s (these are folder names).
        c.  Compile an ordered list of these: `Selected_Addons_AppNames_Ordered`.
    2.  **Define Known Primary Directive Add-on Apps:**
        `Known_Primary_Directive_Addon_Apps`: A list of `addon_app_name`s recognized as primary directives (e.g., `["dev_planner", "create_research_report"]`).
    3.  **Initialize Collections for Add-ons:**
        *   `Active_Primary_Addon_AppName`: null
        *   `All_Selected_Addons_Content_Map`: An empty map for `addon_app_name: primary_instruction_content`.
        *   `App_Specific_Configs_Content_Map`: An empty map for `component_name: raw_config_block_string`. (Note: This map is now generic for any component type, not just add-ons).
        *   `Inheritable_Addons_Content_Ordered_List`: An empty ordered list for content of selected non-primary add-ons.
        *   `Loaded_Primary_Addon_AppNames`: An empty list to track selected primary add-on apps that were successfully loaded.
    4.  **Load Selected Add-on Content & Discover App-Specific Configs:**
        For each `selected_app_name` in `Selected_Addons_AppNames_Ordered`:
        a.  Construct the path to its primary instruction file: `path_to_primary_instruction_file = promptu/components/add_ons/[selected_app_name]/[selected_app_name].txt`.
        b.  Attempt to read the content of this file.
        c.  If successfully read:
            i.  Store its content in `All_Selected_Addons_Content_Map` with `selected_app_name` as key.
            ii. If `selected_app_name` is in `Known_Primary_Directive_Addon_Apps`:
                *   Add `selected_app_name` to `Loaded_Primary_Addon_AppNames`.
                *   If `Active_Primary_Addon_AppName` is still null, set `Active_Primary_Addon_AppName = selected_app_name`.
            iii. **Scan Main Prompt for App-Specific Configuration Block:**
                *   Define delimiters: `start_delim = "[[USER_CONFIG_FOR_" + selected_app_name + "]]"`, `end_delim = "[[END_USER_CONFIG_FOR_" + selected_app_name + "]]"`.
                *   Search the *entire original user-provided main prompt file content* (typically `post_promptu.txt`) for text between these delimiters.
                *   If found, extract the raw string content and store it in `App_Specific_Configs_Content_Map` with `selected_app_name` as the key.
        d.  If the primary instruction file is not found or unreadable: Prepare a warning: "Warning: Selected add-on app '[selected_app_name]' is missing its primary instruction file at '[path_to_primary_instruction_file]' and will be skipped."
    5.  **Populate Inheritable Add-ons List:** (This logic will be primarily driven by promptApp context or if no promptApp is selected)
        For each `loaded_app_name` in `Selected_Addons_AppNames_Ordered` (iterate based on original selection order, but only consider apps successfully loaded):
        a.  If `loaded_app_name` is NOT equal to `Active_Primary_Addon_AppName` (and no promptApp is active, or if promptApp allows), add its content to `Inheritable_Addons_Content_Ordered_List`.
    6.  **Handle Multiple Selected Primary Add-ons:** If `Loaded_Primary_Addon_AppNames` contains more than one app name, prepare a warning.
    7.  The collections are used in Section II.

**Implementation Notes:**
- This is a key initialization procedure.
- Involves parsing `post_promptu.txt` for `[[USER_ADDON_SELECTION]]`.
- Loads add-on instructions from `promptu/components/add_ons/`.
- Identifies primary vs. inheritable add-ons.
- Scans the main prompt (e.g. `post_promptu.txt`) for `[[USER_CONFIG_FOR_addon_app_name]]` blocks.
- Populates several critical data collections for later use in execution logic.
- Includes specific warning conditions.
EOL
