---
id: Trait_Core_ExecutePromptAppPath
name: "PromptApp Execution Path Logic"
category: "core_execution_logic"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "II.A.1"
description: "Defines the primary execution path if a PromptApp is selected and has tasks. Includes task identification, component resolution, loading, parameter resolution, execution, and iteration handling."
strictness_default: "Rule"
version: "1.0"
keywords: ["execution logic", "promptapp", "task execution", "component loading", "parameter resolution", "iteration"]
related_traits: ["Trait_Init_ParseUserPromptAppSelection", "Trait_Core_ComponentResolutionSearchOrder"]
---
**Full Directive Text:**
1.  **IF `Selected_PromptAppName` is set AND `Available_PromptApps_Manifest_Map` contains `Selected_PromptAppName` AND `Selected_App_Tasks_Ordered` is not empty:**
    a.  **PromptApp Execution Path:**
        i.   INFO "Executing selected promptApp: [Selected_PromptAppName]".
        ii.  Get the manifest: `current_app_manifest = Available_PromptApps_Manifest_Map[Selected_PromptAppName]`.
        iii. Identify the *first task name* from `Selected_App_Tasks_Ordered`. Let this be `current_task_user_name`.
        iv.  Find `current_task_info` from `current_app_manifest.phases.tasks` where `task.taskName` matches `current_task_user_name`.
        v.   If `current_task_info` is not found in the manifest: CRITICAL ERROR "Selected task '[current_task_user_name]' for promptApp '[Selected_PromptAppName]' not found in its manifest. Halting." HALT.
        vi.  Let `component_name_to_load = current_task_info.componentName`.
        vii. INFO "Current promptApp task: '[current_task_user_name]' using component: '[component_name_to_load]'".

        **NEW_HANDOFF_LOGIC_STEP_1: Generate ComponentInstanceID**
        *   Invoke `[[USE_TRAIT Trait_Core_GenerateComponentInstanceID]]` with inputs:
            *   `appInstanceID: session.AppInstanceID` (Assumed to be available in the session context)
            *   `componentName: component_name_to_load`
            *   `taskIdentifier: current_task_user_name` (or a task index if names are not unique)
        *   Store the returned unique ID as `currentTask.ComponentInstanceID`.
        *   INFO "Generated ComponentInstanceID: [currentTask.ComponentInstanceID] for task [current_task_user_name]".

        viii. **Component Resolution (Search Order - App-Specific First):**
            *   Path Attempt 1 (App-Local Flat File Style): `path_to_component = promptu/components/apps/[Selected_PromptAppName]/[component_name_to_load].txt`
            *   Path Attempt 2 (App-Local Folder Style): `path_to_component = promptu/components/apps/[Selected_PromptAppName]/[component_name_to_load]/[component_name_to_load].txt` (if not found in app-local flat file)
            *   Path Attempt 3 (Add-on): `path_to_component = promptu/components/add_ons/[component_name_to_load]/[component_name_to_load].txt` (if not found previously)
            *   Path Attempt 4 (Util): `path_to_component = promptu/components/utils/[component_name_to_load]/[component_name_to_load].txt` (if not found previously)
            *   If component file not found after all attempts: CRITICAL ERROR "Component '[component_name_to_load]' for task '[current_task_user_name]' in promptApp '[Selected_PromptAppName]' not found. Search paths were: [list attempted paths]. Halting." HALT.

        ix.  **Load Component & Its Config:**
            *   Read the `component_instruction_content` from the resolved `path_to_component`.
            *   Determine `base_path_of_resolved_component` (e.g., `promptu/components/add_ons/[component_name_to_load]/` or `promptu/components/apps/[Selected_PromptAppName]/`).
            *   Read `user_component_config_content` from `[base_path_of_resolved_component]user_[component_name_to_load]_config.txt`. (Handle file not found gracefully for components that don't have one).
            *   Get `main_prompt_component_config_block_content` from `App_Specific_Configs_Content_Map[component_name_to_load]` (this was populated in I.D.4.c.iii if a `[[USER_CONFIG_FOR_componentName]]` block exists in the main prompt).

        **NEW_HANDOFF_LOGIC_STEP_2: Prepare Component Configuration for Injection**
        *   Let `effective_defaultConfigOverrides` be a mutable copy of `current_task_info.defaultConfigOverrides`.
        *   Add or update the key `COMPONENT_INSTANCE_ID` in `effective_defaultConfigOverrides` with the value `currentTask.ComponentInstanceID`.
        *   This `effective_defaultConfigOverrides` map will be used in step `x.3` below.
        *   INFO "Injected COMPONENT_INSTANCE_ID into effective defaultConfigOverrides for [component_name_to_load]".

        **NEW_HANDOFF_LOGIC_STEP_3: Record Component Invocation in App Handoff Note**
        *   Invoke `[[USE_TRAIT Trait_Core_ManageAppHandoffNote.record_component_invocation]]` with inputs:
            *   `appInstanceID: session.AppInstanceID`
            *   `appName: session.AppName` (Assumed to be available in the session context)
            *   `componentInstanceID: currentTask.ComponentInstanceID`
            *   `componentName: component_name_to_load`
            *   `componentStatus: "Preparing to execute"`
            *   `invocationTimestamp: [Current Timestamp]`
        *   INFO "Recorded component invocation for [currentTask.ComponentInstanceID] in app handoff note."

        x.   **Parameter Resolution for the Component:**
            *   (This adapts the detailed loop previously in add-on logic, like `build_product_specs_process` or `create_research_report` Phase 1).
            *   The component's `user_..._config.txt` defines expected parameters, their Requirement, DefaultType, Description, and Default value.
            *   For each parameter defined in `user_component_config_content`:
                1.  Value from `main_prompt_component_config_block_content` (parsed from `[[USER_CONFIG_FOR_component_name_to_load]]`).
                2.  User-edited value in `user_component_config_content` (parsed from `[USER_VALUE_START]...[USER_VALUE_END]`).
                3.  Value from `effective_defaultConfigOverrides[ParameterName]` (this map now includes `COMPONENT_INSTANCE_ID`).
                4.  Accepted default value in `user_component_config_content` (from `# Default: [value]` if `DefaultType: Accepted`).
                5.  If still unresolved & `Requirement: Required` & `DefaultType: Placeholder`: AI must ask user for value via chat. Update the on-disk `user_..._config.txt` and advise user to update their main prompt's `[[USER_CONFIG_FOR_...]]` block.
                6.  If still unresolved & `Requirement: Required`: CRITICAL ERROR "Required parameter '[ParameterName]' for component '[component_name_to_load]' could not be resolved. Halting." HALT.
            *   Store resolved parameters internally for the component's use.

        xi.  **Execute Component's Instructions:** Execute the `component_instruction_content` using its resolved parameters. (The component is now expected to use the injected `COMPONENT_INSTANCE_ID` for its own handoff/resumability if designed to do so).

        **NEW_HANDOFF_LOGIC_STEP_4: Update Component Status in App Handoff Note (Post-Execution/Status Change)**
        *   After the component task finishes (successfully or with failure), or if its status changes significantly during a long execution (e.g., to "AwaitingUserInput", "Paused"), its actual status MUST be captured.
        *   Invoke `[[USE_TRAIT Trait_Core_ManageAppHandoffNote.update_component_status_in_app_handoff]]` with inputs:
            *   `appInstanceID: session.AppInstanceID`
            *   `appName: session.AppName`
            *   `componentInstanceID: currentTask.ComponentInstanceID`
            *   `newStatus: [Actual component status, e.g., "Completed", "Failed: Specific Error", "Paused"]`
        *   INFO "Updated component status for [currentTask.ComponentInstanceID] to [Actual component status] in app handoff note."

        xii. **Iteration Handling (if `current_task_info.isIterable` is true):**
            *   The executed component is expected to contain logic that, upon its own completion, prompts the user with options like "repeat this task" or "move on to the next task".
            *   If user chooses "move on": The component's instructions should lead to the AI outputting a message like: "Task '[current_task_user_name]' of promptApp '[Selected_PromptAppName]' is complete. To proceed, please reconfigure your main prompt to select the next task: '[Name of next task in Selected_App_Tasks_Ordered, if any; else "No further tasks selected in the current promptApp configuration."]' and then start a new AI session." Then the AI should HALT.
            *   (The MPS framework itself does not manage the task-to-task transition beyond this instruction; user re-invocation with an updated prompt is required for the next task in the `promptApp`.)

**Implementation Notes:**
- This is the main path if a PromptApp is selected.
- Involves:
    - Identifying the current task from `Selected_App_Tasks_Ordered` and manifest.
    - Resolving component location using search order (II.A.1.viii).
    - Loading component instructions and its `user_..._config.txt`.
    - Resolving parameters using values from main prompt config block, user_config, manifest overrides, and defaults. May require asking user if required parameter is missing.
    - Executing the component.
    - Handling iterable tasks by prompting user and halting for re-invocation.
- Critical error handling at multiple points.
EOL
