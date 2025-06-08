---
id: Trait_Core_ComponentResolutionSearchOrder
name: "Component Resolution Search Order"
category: "core_execution_logic"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "II.A.1.viii"
description: "Defines the specific search order for resolving component paths: App-Local Flat, App-Local Folder, Add-on, Util. Critical error if not found."
strictness_default: "Rule"
version: "1.0"
keywords: ["execution logic", "component loading", "path resolution", "search order"]
related_traits: ["Trait_Core_ExecutePromptAppPath"]
---
**Full Directive Text:**
viii. **Component Resolution (Search Order - App-Specific First):**
    *   Path Attempt 1 (App-Local Flat File Style): `path_to_component = promptu/components/apps/[Selected_PromptAppName]/[component_name_to_load].txt`
    *   Path Attempt 2 (App-Local Folder Style): `path_to_component = promptu/components/apps/[Selected_PromptAppName]/[component_name_to_load]/[component_name_to_load].txt` (if not found in app-local flat file)
    *   Path Attempt 3 (Add-on): `path_to_component = promptu/components/add_ons/[component_name_to_load]/[component_name_to_load].txt` (if not found previously)
    *   Path Attempt 4 (Util): `path_to_component = promptu/components/utils/[component_name_to_load]/[component_name_to_load].txt` (if not found previously)
    *   If component file not found after all attempts: CRITICAL ERROR "Component '[component_name_to_load]' for task '[current_task_user_name]' in promptApp '[Selected_PromptAppName]' not found. Search paths were: [list attempted paths]. Halting." HALT.

**Implementation Notes:**
- This rule is applied when loading a component specified by a PromptApp task.
- The search order is critical:
    1. `promptu/components/apps/[Selected_PromptAppName]/[component_name_to_load].txt`
    2. `promptu/components/apps/[Selected_PromptAppName]/[component_name_to_load]/[component_name_to_load].txt`
    3. `promptu/components/add_ons/[component_name_to_load]/[component_name_to_load].txt`
    4. `promptu/components/utils/[component_name_to_load]/[component_name_to_load].txt`
- If the component is not found after checking all paths, it's a critical error, and the AI must HALT.
EOL
