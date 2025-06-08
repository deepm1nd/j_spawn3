---
id: Trait_Init_DiscoverLoadUtils
name: "Discover and Load Utility Components (Utils)"
category: "initialization_config"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "I.H"
description: "Scans for and loads utility components from promptu/components/utils/, handling both subdirectory and direct .txt file styles. Prioritizes subdirectory versions in case of name collision."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "utils", "utilities", "component loading", "discovery"]
related_traits: []
---
**Full Directive Text:**
H. **Discover and Load Utility Components ("Utils"):**
    a.  Initialize `All_Available_Utils_Content_Map` (map for `util_app_name: content`).
    b.  Scan for all subdirectories within `promptu/components/utils/`. For each `util_dir_name`:
        i.  Define `util_app_name = util_dir_name`.
        ii. Path to component: `path_to_util = promptu/components/utils/[util_dir_name]/[util_dir_name].txt`.
        iii. Attempt to load content from `path_to_util`. If successful, store it in `All_Available_Utils_Content_Map` with `util_app_name` as key and mark this `util_app_name` as "loaded_from_subdir".
    c.  Scan for all `.txt` files directly within `promptu/components/utils/` (e.g., `utility_name.txt`). For each `util_file_name.txt`:
        i.  Define `util_app_name = util_file_name` (without the .txt extension).
        ii. If `util_app_name` is already in `All_Available_Utils_Content_Map` (i.e., "loaded_from_subdir"), issue a warning: "Warning: Utility '[util_app_name]' found both as direct file `promptu/components/utils/[util_file_name].txt` and in subdirectory `promptu/components/utils/[util_app_name]/`. Prioritizing version from subdirectory. Please ensure uniqueness." and SKIP this file.
        iii. Else:
            *   Path to component: `path_to_util = promptu/components/utils/[util_file_name].txt`.
            *   Attempt to load content from `path_to_util`. If successful, store it in `All_Available_Utils_Content_Map` with `util_app_name` as key.
    d.  For any util not loaded successfully (either type), issue appropriate warnings.
    e.  Utils are available for use by other components.

**Implementation Notes:**
- Scan `promptu/components/utils/` for components.
- Supports two structures for utils:
    1. `utils/[util_name]/[util_name].txt`
    2. `utils/[util_name].txt`
- If a util exists in both forms, the subdirectory version is prioritized.
- Loaded utils are stored in `All_Available_Utils_Content_Map`.
- Issue warnings for loading failures or name collisions.
EOL
