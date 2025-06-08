---
id: Trait_DevPlanner_WorkflowDelegatedPlanning
name: "DevPlanner - Workflow Delegated Planning"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "II.0.f" # DelegatedPlanning part
description: "Defines the DelegatedPlanning workflow: analyze project for decomposition (2-5 modules), define planning sub-tasks for each, generate prompts for Planning Task AIs (including KB usage for interface design), update TLP, and handoff for user review and sub-plan merging."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "workflow", "delegated planning", "decomposition", "sub-planning", "task generation", "handoff"]
related_traits: ["Trait_DevPlanner_ApplyInterfaceBasedDesignPrinciples", "Trait_DevPlanner_TaskPromptFileStructure", "Trait_DevPlanner_TLPFileStructure", "Trait_DevPlanner_ConflictAndAmbiguityResolutionProtocol"]
---
**Full Directive Text:**
(Section II.0.f selector for DelegatedPlanning workflow:
    IF `Resolved_DP_Workflow_Type` is "Monolithic": ...
    ELSE IF `Resolved_DP_Workflow_Type` is "DelegatedPlanning":
        INFO: "DelegatedPlanning workflow selected. This workflow is typically used for initial detailed planning of the entire project or a major segment, focusing on decomposition into plannable sub-modules."
        1.  **Analyze Project for Planning Decomposition:** Based on `User_High_Level_Project_Goal`, the enhanced initial research report (from Section II.0.B), and a thorough analysis of *all documents/URLs* within `Resolved_DP_Project_Specifications_Ref` (if provided), identify 2-5 major components, modules, or work areas.
            *   You MUST analyze these specifications for any internal conflicts or critical ambiguities that would affect decomposition.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
            *   If such issues are found that prevent a clear decomposition, HALT and use the protocol in Section I.E to query the user.
            *   All decomposition decisions MUST be demonstrably consistent with the (clarified, if necessary) `Resolved_DP_Project_Specifications_Ref`. These areas require separate, detailed sub-planning.
        2.  **Define Planning Sub-Tasks:** For each identified component/module:
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
            a.  Formulate a clear objective for a "Planning Task AI". (Objective text is extensive, includes adherence to specs, exemplars, methodology guides, and interface-based design via KBs - see full text in source.)
            b.  This Planning Task AI MAY be authorized to conduct further focused research if essential for its sub-planning task. (Configuration details for research are specified in source.)
            c.  The primary output for each Planning Task AI is a Markdown sub-plan document (e.g., `[ComponentName]_sub_plan.md`) to be saved in `[Resolved_DP_Submodule_Plans_Subdir]/planning_phase/`. The content and structure of this document should be informed by exemplars in `[Resolved_DP_Plan_Exemplars_Dir]` (it is in the repo).
        3.  **Generate Prompts for Planning Task AIs:** For each planning sub-task, generate a dedicated task prompt file in `[Resolved_DP_Task_Prompts_Subdir]/planning_phase/` using the naming convention `p0_planning_t[N]_[component_name].txt`. These prompts MUST explicitly reiterate the requirement for the Planning Task AI to analyze and adhere to all documents in `[Resolved_DP_Project_Specifications_Ref]` and consult `[Resolved_DP_Plan_Exemplars_Dir]`, including the conflict/ambiguity analysis duty. They should follow the general structure outlined in Section III.B of this document, but with the "Task Execution Instructions" focused on research (if enabled) and detailed sub-plan document creation.
        4.  **Update Task Launch Plan (TLP):** Modify the `00_task_launch_plan.md` (as defined in Section III.C) to:
            a.  Clearly state that the first phase is "Phase 0: Delegated Planning".
            b.  List all generated "Planning Task AI" prompts with their objectives and launch commands.
            c.  Add a section after Phase 0 tasks: "**User Action Required: Plan Merging & Approval.** After all Planning Task AIs complete, their generated sub-plans (located in `[Resolved_DP_Submodule_Plans_Subdir]/planning_phase/`) must be reviewed and merged by the User (or a dedicated Plan Merging AI task, if developed) into the main `[Resolved_DP_Main_Dev_Plan_Filename]`. This merged and approved Main Development Plan will be the input for subsequent developer task spawning."
        5.  **Prepare for Handoff/Continuation:**
            a.  The `dev_planner`'s current execution effectively concludes after generating these planning tasks and the updated TLP.
            b.  The AI (running `dev_planner`) MUST ensure the main session `handoff_notes.md` is updated to reflect:
                i.  Current state: `dev_planner` (DelegatedPlanning workflow) has spawned planning sub-tasks.
                ii. Path to the TLP: `[Resolved_DP_Task_Prompts_Subdir]/00_task_launch_plan.md`.
                iii. Paths to expected sub-plan outputs: `[Resolved_DP_Submodule_Plans_Subdir]/planning_phase/`.
                iv. Clear instructions that the next overall step is User review and merging of sub-plans into `[Resolved_DP_Main_Dev_Plan_Filename]`.
                v.  Indication that `dev_planner` (or a different orchestrator) can be re-invoked with the finalized `[Resolved_DP_Main_Dev_Plan_Filename]` to proceed with spawning developer tasks for project execution.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
            c.  HALT execution of this `dev_planner` instance. The framework will then proceed based on the updated handoff notes (e.g., by awaiting user action or prompting for the next step).
    ELSE IF `Resolved_DP_Workflow_Type` is "HybridIterative": ...
)

**Implementation Notes:**
- Used for initial detailed planning of project/major segment, decomposing into 2-5 modules.
- Key Steps:
    1. Analyze project for decomposition (conflict check with specs).
    2. Define planning sub-tasks for each module (includes extensive instructions for Planning Task AI on using KBs for interface design).
    3. Generate prompts for these Planning Task AIs.
    4. Update TLP, stating "Phase 0: Delegated Planning" and listing planning tasks. Crucially, TLP must note "User Action Required: Plan Merging & Approval" into `MAIN_DEV_PLAN_FILENAME`.
    5. Handoff, indicating next step is user review and merging of sub-plans. `dev_planner` HALTS.
- Resumable session handoff updates are noted at HALT points.
EOL
