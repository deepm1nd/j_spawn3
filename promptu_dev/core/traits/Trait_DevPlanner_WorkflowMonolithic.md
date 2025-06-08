---
id: Trait_DevPlanner_WorkflowMonolithic
name: "DevPlanner - Workflow Monolithic"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "II.1" # Also see II.0.f for entry condition
description: "Defines the Monolithic workflow: decompose project/phase into modules, create main overview doc, generate sub-planning task prompts, handoff for user review, then resume to generate developer tasks from completed/approved submodule plans. Includes fallback for critical issues."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "workflow", "monolithic", "decomposition", "sub-planning", "task generation", "handoff"]
related_traits: ["Trait_DevPlanner_ApplyInterfaceBasedDesignPrinciples", "Trait_DevPlanner_MainDevPlanDocStructure", "Trait_DevPlanner_TaskPromptFileStructure", "Trait_DevPlanner_TLPFileStructure", "Trait_DevPlanner_ConflictAndAmbiguityResolutionProtocol"]
---
**Full Directive Text:**
(Section II.0.f selector for Monolithic workflow:
    IF `Resolved_DP_Workflow_Type` is "Monolithic":
        // Proceed with instructions in Section II.1 (Monolithic Workflow)
        Proceed with instructions in Section II.1 onwards.
    ELSE IF `Resolved_DP_Workflow_Type` is "DelegatedPlanning": ...
    ELSE IF `Resolved_DP_Workflow_Type` is "HybridIterative": ...
    ELSE (Unknown workflow type):
        WARNING: "Unknown WORKFLOW_TYPE: '[Resolved_DP_Workflow_Type]'. Defaulting to Monolithic workflow."
        Set `Resolved_DP_Workflow_Type` internally to "Monolithic" and proceed with Section II.1.
)

**II.1. MONOLITHIC WORKFLOW: Project Decomposition, Sub-Planning, and Developer Task Generation**
    **Phase Scope Note:** If this workflow is invoked for a specific phase (see II.0.A), the "project" decomposition below should focus on sub-modules *within* that phase. If for initial overall planning, it addresses the entire project.

1.  **Decompose Project into Modules:**
    Based on `User_High_Level_Project_Goal` (potentially phase-scoped), the enhanced initial research report (from Section II.0.B), and critically, a thorough analysis of *all documents/URLs* within `Resolved_DP_Project_Specifications_Ref` (if provided, it is in the repo), decompose the overall project (or current phase) into a list of 2-5 logical/functional modules.
    *   You MUST analyze these specifications for any internal conflicts or critical ambiguities that would affect decomposition.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
    *   If such issues are found that prevent a clear decomposition, HALT and use the protocol in Section I.E to query the user.
    *   All module definitions, boundaries, and high-level responsibilities MUST be demonstrably consistent with the (clarified, if necessary) `Resolved_DP_Project_Specifications_Ref`. Each module will have its own submodule development plan.

2.  **Create/Update Main Overview Document (`MAIN_DEV_PLAN_FILENAME`):**
    Create or update the document specified by `[Resolved_DP_Main_Dev_Plan_Filename]` (e.g., `main_dev_plan_overview.md`). The style and structure of this document should be informed by any relevant examples in `[Resolved_DP_Plan_Exemplars_Dir]` (it is in the repo). This document MUST:
    a.  Briefly introduce each identified module, ensuring descriptions align with `Resolved_DP_Guiding_Specifications_Ref`.
    b.  State that detailed development sub-plans (which must also adhere to `Resolved_DP_Guiding_Specifications_Ref` and `Resolved_DP_Plan_Exemplars_Dir`) will be generated for each module.
    c.  Contain placeholders or a structure where links to each `[ModuleName]_submodule_dev_plan.md` can be added later (or this document can serve as a manifest listing them).

3.  **Generate Sub-Planning Task Prompts:**
    For each identified module:
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
    a.  Define an objective for a "Sub-Planning Task AI" to create a detailed `[ModuleName]_submodule_dev_plan.md`. (Objective text is extensive, includes adherence to specs, exemplars, methodology guides, and interface-based design via KBs - see full text in source.)
    b.  This Sub-Planning Task AI MAY be authorized for further focused research. (Configuration details for research are specified in source.)
    c.  The primary output `[ModuleName]_submodule_dev_plan.md` is to be saved in `[Resolved_DP_Submodule_Plans_Subdir]/`.
    d.  Generate a task prompt file (e.g., `[Resolved_DP_Task_Prompts_Subdir]/module_planning_[ModuleName].txt`) for this sub-planning task, following the structure in Section III.B, tailored for planning.

4.  **Update Task Launch Plan (TLP) for Sub-Planning Phase:**
    Modify `00_task_launch_plan.md` (as per Section III.C) to:
    a.  Clearly title this phase, e.g., "Phase 0: Project Decomposition and Sub-Planning Task Generation".
    b.  List all generated "Sub-Planning Task AI" prompts, their objectives, and launch commands.

5.  **Handoff for User Review of Decomposition & Sub-Planning Tasks:**
    The AI (running `dev_planner`) MUST ensure the main session `handoff_notes.md` is updated to reflect:
    - Current state: `dev_planner` (Monolithic workflow) has produced an initial project decomposition and generated tasks for detailing submodule plans.
    - `Current_Workflow_Step`: "AwaitingDecompositionAndSubPlanTaskApproval".
    - Paths to review and next user action.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
    - HALT execution.

6.  **Resumption: Generate Developer Tasks from Completed Submodule Plans:**
    This section executes if `dev_planner` is re-invoked and handoff state indicates "AwaitingDeveloperTaskGenerationAfterSubplans".
    a.  **Update Overview Document:** Link/incorporate summaries from each `[ModuleName]_submodule_dev_plan.md` into the main overview document.
    b.  **Developer Task Generation:**
        i.   Infer purpose of methodology guides.
        ii.  Iterate through each approved `[ModuleName]_submodule_dev_plan.md`:
            1.  Decompose into granular developer tasks, ensuring adherence to specs.
            2.  Apply merge conflict mitigation.
            3.  Generate developer task prompts, instructing Task AIs to adhere to specs and relevant methodology guides.
    c.  **Update TLP for Developer Tasks:** Update `00_task_launch_plan.md`.
    d.  **Final Handoff for Execution:** Update handoff notes.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
    - HALT.
    **Monolithic Workflow Fallback/Critical Issue Handling:**
    If critical planning failures or unresolvable blockers occur:
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
    a.  HALT current planning.
    b.  Summarize issues in handoff notes.
    c.  Suggest re-attempting scope with DelegatedPlanning or HybridIterative workflow.

**Implementation Notes:**
- This is a major workflow for `dev_planner`.
- Can be phase-scoped or project-scoped.
- Key stages:
    1. Decompose project/phase into modules (conflict check with specs).
    2. Create/update main overview document.
    3. Generate sub-planning task prompts for each module (includes extensive instructions on using KBs for interface design).
    4. Update TLP for this sub-planning phase.
    5. Handoff for user review and approval of decomposition and sub-planning tasks.
    6. Resume to generate developer tasks from approved submodule plans (again, emphasizing spec adherence and methodology guides).
    7. Update TLP for developer tasks.
    8. Final handoff for execution.
- Includes fallback to suggest other workflows if Monolithic planning is blocked.
- Resumable session handoff updates are noted at HALT points.
EOL
