---
id: Trait_DevPlanner_WorkflowHybridIterative
name: "DevPlanner - Workflow Hybrid Iterative"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "II.0.f" # HybridIterative part
description: "Defines the HybridIterative workflow: resumable, stateful planning and execution one phase/module at a time. Involves steps like PlanCurrentPhase, AwaitingSubPlanApproval, ReadyToSpawnDevTasks, AwaitingPhaseCompletion, and looping or concluding."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "workflow", "hybrid iterative", "resumable", "stateful", "phased execution", "sub-planning", "task generation", "handoff"]
related_traits: ["Trait_DevPlanner_ApplyInterfaceBasedDesignPrinciples", "Trait_DevPlanner_TaskPromptFileStructure", "Trait_DevPlanner_TLPFileStructure", "Trait_Core_AIdentity_UpdateHandoffNotes"]
---
**Full Directive Text:**
(Section II.0.f selector for HybridIterative workflow:
    IF `Resolved_DP_Workflow_Type` is "Monolithic": ...
    ELSE IF `Resolved_DP_Workflow_Type` is "DelegatedPlanning": ...
    ELSE IF `Resolved_DP_Workflow_Type` is "HybridIterative":
        INFO: "HybridIterative workflow selected."
        // This workflow requires dev_planner to be resumable and stateful via main handoff notes.
        // It will manage planning and execution one phase/module at a time, making it suitable for iterative execution or for handling specific, complex phases identified by PromptuDev_AI.

        1.  **Determine Current Iteration State:**
            a.  Check main session `handoff_notes.md` for a "Pending Task" entry related to this `dev_planner` (HybridIterative) instance.
            b.  If found, parse it to determine: `Current_Phase_Or_Module_ID`, `Current_Iteration_Step` (e.g., "AwaitingSubPlanApproval", "ReadyToSpawnDevTasks", "AwaitingPhaseCompletion"), paths to relevant plans.
            c.  If not found (first invocation): Set `Current_Phase_Or_Module_ID` (from `User_High_Level_Project_Goal`/research), `Current_Iteration_Step` to "PlanCurrentPhase".

        2.  **Execute Current Iteration Step:**

            *   **IF `Current_Iteration_Step` is "PlanCurrentPhase":**
                a.  Focused planning for `Current_Phase_Or_Module_ID` (conflict check with specs).
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
                b.  Generate Planning Task AI prompt for sub-plan (includes extensive instructions on using KBs for interface design).
                c.  Update TLP.
                d.  Handoff: `dev_planner` paused, `Current_Iteration_Step` = "AwaitingSubPlanApproval". User to review/approve sub-plan.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
                e.  HALT.

            *   **ELSE IF `Current_Iteration_Step` is "AwaitingSubPlanApproval":**
                a.  Instruct user to finalize sub-plan and re-invoke.
                b.  Handoff: Awaiting user finalization. `Current_Iteration_Step` may move to "ReadyToSpawnDevTasks" upon user confirmation.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
                d.  HALT.

            *   **ELSE IF `Current_Iteration_Step` is "ReadyToSpawnDevTasks":**
                a.  Load approved sub-plan (verified for spec compliance).
                b.  Generate Developer Task AI prompts for the phase/module (instructing spec adherence and methodology guide usage).
                c.  Update TLP.
                d.  Handoff: `dev_planner` spawned dev tasks. `Current_Iteration_Step` = "AwaitingPhaseCompletion". User to execute tasks and merge.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
                e.  HALT.

            *   **ELSE IF `Current_Iteration_Step` is "AwaitingPhaseCompletion":**
                a.  Instruct user: phase tasks defined, execute/merge, then re-invoke `dev_planner`.
                b.  Handoff: `Current_Iteration_Step` remains "AwaitingPhaseCompletion".
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
                c.  HALT.

        3.  **Loop or Conclude:** (Logic for when `dev_planner` is re-invoked after "AwaitingPhaseCompletion")
            a.  Determine if there is a next phase/module.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
            b.  If yes: Set next `Current_Phase_Or_Module_ID`, `Current_Iteration_Step` to "PlanCurrentPhase". Update handoff. HALT (to start new cycle).
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
            c.  If no: Project complete. Update handoff. HALT.
)

**Implementation Notes:**
- This workflow is stateful and resumable, managed via handoff notes.
- Processes one phase/module at a time.
- Key steps:
    1. Determine current state from handoff notes (`Current_Phase_Or_Module_ID`, `Current_Iteration_Step`).
    2. Execute logic for the current step:
        - "PlanCurrentPhase": Plan current phase, generate planning task, handoff for sub-plan approval.
        - "AwaitingSubPlanApproval": Instruct user to finalize sub-plan, handoff.
        - "ReadyToSpawnDevTasks": Load approved sub-plan, generate developer tasks, handoff for phase execution.
        - "AwaitingPhaseCompletion": Instruct user phase tasks are ready, execute/merge, then re-invoke; handoff.
    3. Loop to next phase or conclude project upon re-invocation after phase completion.
- Each step involves updating handoff notes and HALTING.
- Resumable session handoff updates are noted at HALT points.
EOL
