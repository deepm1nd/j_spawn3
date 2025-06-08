---
id: Trait_AIdentityQB_PhasedDevPhase1ToN
name: "AIdentity QB - Phased Development Process (Phase 1...N Logic)"
category: "aidentity_lifecycle"
source_file: "promptu_dev/aidentity/promptu_qb.txt"
source_section: "Phase 1...N: Iterative Planning, Execution, Acceptance"
description: "Defines the iterative logic for Phases 1 to N: identify current phase objective, detailed phase planning via dev_planner, handoff for user task execution, resume for acceptance analysis via dev_planner, and phase completion/remediation loop."
strictness_default: "Guideline"
version: "1.0"
keywords: ["aidentity", "phased development", "iterative process", "planning", "execution", "acceptance", "remediation", "dev_planner"]
related_traits: ["Trait_AIdentityQB_PhasedDevPhase0", "Trait_AIdentityQB_PhasedDevProjectCompletion"]
---
**Full Directive Text:**
**Phase 1...N: Iterative Planning, Execution, Acceptance**
(This logic is executed if handoff indicates resuming a specific phase, or after Phase 0 completes)
1.  **Identify Current Phase:** Determine `Current_Phase_Objective` from `Final_Phase_Structure` at `Current_Phase_Index`.
2.  **Detailed Phase Planning (Invoke `dev_planner`):**
    *   Prepare a `User_High_Level_Project_Goal` for `dev_planner` specific to `Current_Phase_Objective` (e.g., "Generate detailed plan for Phase: [Current_Phase_Objective]").
    *   Invoke `dev_planner` using this phase-specific goal and the overall `[[USER_CONFIG_FOR_dev_planner]]`.
    *   `dev_planner` will perform its full process (research, spec adherence with its internal conflict/ambiguity checks as per its Section I.E, exemplars, methodology guides) and produce phase sub-plans, task TLP, task prompts, and user documentation for this phase.
    *   **Monolithic Workflow Nuance:** If `dev_planner` (running Monolithic) HALTS due to critical internal planning issues it cannot resolve:
        *   Summarize the issues to the user.
        *   Offer the user the option to re-attempt planning for this phase by re-invoking `dev_planner` with `WORKFLOW_TYPE` set to `DelegatedPlanning` or `HybridIterative`.
        *   Await user decision. If they choose to switch, update `[[USER_CONFIG_FOR_dev_planner]]` for the *next* `dev_planner` run for this phase accordingly.
    *   Facilitate any user reviews of the generated plans as per `dev_planner`'s workflow.
3.  **Handoff for User Task Execution:**
    *   Once detailed plans and task prompts for `Current_Phase_Objective` are finalized and approved by user, record this state.
    *   Instruct the user: "Phase [Current_Phase_Index+1]: [Current_Phase_Objective] is planned. Task prompts are available at [path to dev_planner task prompts output for this phase]. Please execute tasks and merge deliverables. Then, use a continuation prompt to resume for Acceptance Analysis."
    *   Generate handoff notes for the next session indicating `Focus: Acceptance Analysis for Phase [Current_Phase_Index+1]`. HALT.
4.  **Acceptance Analysis (Resume here after user continuation_prompt):**
    *   The primary directive from handoff should indicate "Acceptance Analysis for Phase X."
    *   Invoke `dev_planner` in its "Acceptance Analysis Mode." Inputs: `Current_Phase_Objective`, paths to expected deliverables (from previous `dev_planner` TLP), original phase plan, `PROJECT_SPECIFICATIONS_REF`.
    *   `dev_planner` produces an "Acceptance Analysis Report."
5.  **Phase Completion / Remediation Loop:**
    *   Review the Acceptance Analysis Report.
    *   IF all items accepted:
        *   Inform user: "Phase [Current_Phase_Index+1]: [Current_Phase_Objective] accepted."
        *   Increment `Current_Phase_Index`.
        *   If `Current_Phase_Index` < length of `Final_Phase_Structure`: Loop to Step 1 of "Phase 1...N" for the next phase.
        *   Else (all phases done): Proceed to "Project Completion."
    *   ELSE (items not accepted):
        *   Inform user of failed/incomplete items.
        *   Invoke `dev_planner` in its "Remediation Planning Mode." Inputs: Failed items from Acceptance Report, original phase plan, specs.
        *   `dev_planner` generates a focused "Remediation TLP" and task prompts for *only* the outstanding items for the current phase.
        *   Loop to Step 3 of "Phase 1...N" (Handoff for User Task Execution) but with the Remediation TLP.

**Implementation Notes:**
- This is the main loop for iterative development.
- Key steps:
    1. Identify current phase objective.
    2. Invoke `dev_planner` for detailed planning of that phase. Handle monolithic workflow issues by offering user a switch to other workflow types.
    3. Handoff to user for task execution (AI HALTS, session ends).
    4. Upon session resumption (with handoff indicating "Acceptance Analysis"), invoke `dev_planner` for acceptance analysis.
    5. Review acceptance report:
        - If accepted: Increment phase index. If more phases, loop to step 1. If all done, go to Project Completion.
        - If not accepted: Invoke `dev_planner` for remediation planning, then loop to step 3 (Handoff for user execution of remediation tasks).
EOL
