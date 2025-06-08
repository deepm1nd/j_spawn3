---
id: Trait_DevPlanner_TLPFileStructure
name: "DevPlanner - Task Launch Plan (TLP) File Structure"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "III.C.1"
description: "Defines the structure and dynamic content generation for the 00_task_launch_plan.md file, including overview, Mermaid diagram, missing add-on warnings, and dynamic task listing with launch commands."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "output document", "task launch plan", "tlp", "structure", "mermaid diagram", "dynamic content"]
related_traits: ["Trait_DevPlanner_TaskPromptFileStructure", "Trait_DevPlanner_PathConstructionPrinciple"]
---
**Full Directive Text:**
C.  **Supporting Documentation (Task Spawning Specific):**
    1.  **`00_task_launch_plan.md`:** (Note: Generation of `information_exchange_protocol.md` is handled by Core Instructions Section III.C.1, which should place it in `[Resolved_DP_Base_Output_Path]/[Resolved_DP_Task_Prompts_Subdir]/`)
        *   Create based on template. Dynamically replace placeholders.
        *   **Location:** `[Resolved_DP_Base_Output_Path]/[Resolved_DP_Task_Prompts_Subdir]/00_task_launch_plan.md`.
        *   **Launch Commands Path Logic:** Task prompt paths for launch commands will be like `[Resolved_DP_Base_Output_Path]/[Resolved_DP_Task_Prompts_Subdir]/p1_t1_example.txt`. Ensure these are correctly constructed relative to the repository root.
        *   **Missing Add-on Warnings:** If add-ons from `[[USER_ADDON_SELECTION]]` (parsed by Core Instructions Section I.D) were not found, include the warning message (provided by logic in Core Instructions I.D.4.d) at the top of TLP.
        *   **TLP TEMPLATE:** (Full template content as provided in source)
            ```markdown
            # Task Launch Plan for [Resolved_DP_Project_Name] - Iteration [Resolved_DP_Iteration_ID]

            **Overall Planning Confidence:** <High | Medium | Low (P-AI to populate)>
            **Confidence Rationale:** <P-AI to provide a brief explanation...>
            ---
            ## 1. Visual Plan Overview
            ```mermaid
            [[[P-AI TO GENERATE MERMAID FLOWCHART DIAGRAM HERE]]]
            graph TD; A["T1"] --> B["T2"];
            ```
            *(Diagram above is placeholder...)*

            [[[P-AI TO INSERT WARNINGS ABOUT MISSING ADD-ONS HERE, IF ANY]]]
            ---
            ## 1. Overview
            Welcome...
            **Key Files & Folders (relative to repo root):**
            *   Task Prompts, TLP, IEP: `[Resolved_DP_Base_Output_Path]/[Resolved_DP_Task_Prompts_Subdir]/`
            *   Inter-AI Communication (IPC): `[Resolved_DP_Base_Output_Path]/[Resolved_DP_Task_Prompts_Subdir]/ipc/`
            *   Task Sub-Development Plans (`dev_plan.md`, `next_steps.md`): `[Resolved_DP_Base_Output_Path]/[Resolved_DP_Submodule_Plans_Subdir]/`
            *   Task Output Artifacts: `[Resolved_DP_Base_Output_Path]/[Resolved_DP_Task_Artifacts_Subdir]/`
            ## 2. Execution Workflow
            ...
            ## 3. Launching Tasks
            ---
            [[YOU MUST DYNAMICALLY GENERATE THE TASK LISTING BELOW]]
            ...
            ---
            [[END OF DYNAMIC TASK LISTING]]
            ## 4. Iteration Completion
            ...
            ```

**Implementation Notes:**
- The TLP (`00_task_launch_plan.md`) is created from a template with dynamic placeholder replacement.
- Location: `[Resolved_DP_Base_Output_Path]/[Resolved_DP_Task_Prompts_Subdir]/00_task_launch_plan.md`.
- Must correctly construct relative paths for launch commands.
- Includes warnings for missing add-ons at the top.
- Key dynamic sections:
    - Project Name and Iteration ID in title.
    - Planning confidence and rationale.
    - Mermaid flowchart diagram of tasks.
    - Key file/folder paths (using resolved parameters).
    - Dynamic task listing: For each phase and task, include filename, description, effort, dependencies, and launch/retry commands.
- The IEP document should be co-located by the Core Instructions logic.
EOL
