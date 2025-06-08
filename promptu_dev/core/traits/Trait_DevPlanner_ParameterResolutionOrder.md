---
id: Trait_DevPlanner_ParameterResolutionOrder
name: "DevPlanner - Parameter Resolution Order"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "I.B"
description: "Defines the strict order dev_planner uses to resolve its configuration parameters (from [[USER_CONFIG_FOR_dev_planner]], USER_dev_planner_CONFIG.txt, accepted defaults) and how it handles required but unresolved parameters (must ask user)."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "configuration", "parameters", "initialization", "resolution order", "user interaction"]
related_traits: ["Trait_DevPlanner_MandatoryInputConfirmation"]
---
**Full Directive Text:**
B.  **Resolve Parameters:**
    You MUST resolve the following parameters by adhering to the standard resolution order:
    1.  Value from the `[[USER_CONFIG_FOR_dev_planner]]` block (passed in `App_Specific_Configs_Content_Map['dev_planner']`).
    2.  User-edited value in `USER_dev_planner_CONFIG.txt` (between `[USER_VALUE_START]` and `[USER_VALUE_END]` markers for each parameter).
    3.  The "AcceptedDefault" value specified in the comment for that parameter in `USER_dev_planner_CONFIG.txt` (e.g., `# Default: actual_default_value`).
    4.  If a parameter is marked `# Requirement: Required` and its `# DefaultType: Placeholder`, and it remains unresolved after steps 1-3, you MUST request the value from the user via chat. After receiving the value, you MUST update the on-disk `promptu/add_ons/dev_planner/USER_dev_planner_CONFIG.txt` file (specifically the value between `[USER_VALUE_START]` and `[USER_VALUE_END]`) AND advise the user to also update their main prompt's `[[USER_CONFIG_FOR_dev_planner]]` block with this new value for future consistency.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
    5.  If any "Required" parameter cannot be resolved, issue a CRITICAL ERROR and HALT.
    Note: For parameters expected to be a list of paths/URLs (e.g., `PROJECT_SPECIFICATIONS_REF`, `PLAN_METHODOLOGY_GUIDES`, `PLAN_EXEMPLAR_REFS`), the resolved string value (if not empty and not a placeholder like "N/A") should be split by commas to form a list of strings. Trim whitespace from each item in the list. This list is then stored in the corresponding `Resolved_DP_...` variable. If the value is empty or a placeholder, the `Resolved_DP_...` variable should be an empty list or reflect the "N/A" status appropriately.

**Implementation Notes:**
- This defines the core parameter loading sequence for `dev_planner`.
- Strict order: `[[USER_CONFIG_FOR_dev_planner]]` > `USER_dev_planner_CONFIG.txt` user value > `USER_dev_planner_CONFIG.txt` accepted default.
- Handles list-based path parameters by splitting comma-separated strings.
- Includes mandatory user interaction and file update if a required parameter with a placeholder default is not resolved.
- Critical error and HALT if a required parameter remains unresolved.
- Notes resumable session handoff update points.
EOL
