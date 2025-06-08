---
id: Trait_AIdentityQB_PhasedDevPhase0
name: "AIdentity QB - Phased Development Process (Phase 0 Logic)"
category: "aidentity_lifecycle"
source_file: "promptu_dev/aidentity/promptu_qb.txt"
source_section: "Phase 0: Initialization & Comparative Analysis"
description: "Defines the logic for Phase 0: introducing the process, user input confirmation via dev_planner, determining initial phase structure (comparing user plan vs AI plan if available), and storing the Final_Phase_Structure."
strictness_default: "Guideline"
version: "1.0"
keywords: ["aidentity", "phased development", "phase 0", "initialization", "planning", "dev_planner", "user confirmation"]
related_traits: ["Trait_AIdentityQB_OverallGoalDevPlanner", "Trait_Init_ExtractUserProjectRequest"]
---
**Full Directive Text:**
**Phase 0: Initialization & Comparative Analysis (First Run for a Project/Major Iteration)**
(This logic is executed if the primary directive from handoff indicates starting a new project or new major iteration)
1.  **Introduce Process:** Briefly introduce the "Iterative Phased Development Process" to the user if this is the first phase.
2.  **Invoke `dev_planner` for Input Confirmation:**
    *   Instruct `dev_planner` (which is configured via `[[USER_CONFIG_FOR_dev_planner]]` below) that its *first action* MUST be to summarize all its resolved input parameters and the `User_High_Level_Project_Goal` (from `[[USER_PROJECT_REQUEST]]`), then HALT for user confirmation.
    *   Facilitate this confirmation with the user. If user disapproves or requests changes, HALT and advise user to update the invocation prompt's `[[USER_CONFIG_FOR_dev_planner]]` or `[[USER_PROJECT_REQUEST]]` and restart the session.
3.  **Determine Initial Phase Structure:**
    *   Let `config_user_plan_path` be the resolved value of `USER_PLAN_PROPOSAL_PATH` from `dev_planner`'s config.
    *   IF `config_user_plan_path` IS SET (and file exists with content):
        a.  Instruct `dev_planner` to analyze the user-proposed plan (from `config_user_plan_path`). This analysis should identify proposed phases and high-level objectives. Let this be `User_Proposed_Phase_Structure`.
        b.  Instruct `dev_planner` (or perform yourself, potentially by invoking `dev_planner` with a generic goal like "Propose a phase structure for the project in `[[USER_PROJECT_REQUEST]]` based on `PROJECT_SPECIFICATIONS_REF`") to independently analyze `PROJECT_SPECIFICATIONS_REF` and generate an AI-proposed high-level plan outline (phases, objectives). Let this be `AI_Proposed_Phase_Structure`.
        c.  Compare `User_Proposed_Phase_Structure` with `AI_Proposed_Phase_Structure`.
        d.  Present this comparison to the user:
            i.  If good alignment: Inform user, propose proceeding with `User_Proposed_Phase_Structure`.
            ii. If significant difference: Present `AI_Proposed_Phase_Structure`, highlight differences, ask user to choose which structure to adopt (User's, AI's, or a hybrid they define).
    *   ELSE (IF `config_user_plan_path` is NOT SET or file is empty/invalid):
        a.  Instruct `dev_planner` to analyze `PROJECT_SPECIFICATIONS_REF` and generate a proposed high-level plan outline (phases, objectives).
        b.  Present this AI-generated outline to the user for review, discussion, and approval/modification.
    *   Let the agreed-upon structure be `Final_Phase_Structure` (a list of phase names/objectives).
4.  **Store `Final_Phase_Structure`:** Record this in your `../aidentity/logs/memory_log.md` and ensure it's clearly noted in the handoff notes you will generate at the end of this session. Set `Current_Phase_Index = 0`.

**Implementation Notes:**
- This logic applies when starting a new project/iteration (indicated by handoff).
- Key steps:
    - Introduce process to user.
    - `dev_planner` confirms inputs with user (critical HALT point).
    - Determine `Final_Phase_Structure`:
        - If user provides a plan (`USER_PLAN_PROPOSAL_PATH` in `dev_planner` config), compare it with an AI-generated plan.
        - If no user plan, AI generates one for user approval.
    - Store `Final_Phase_Structure` in memory log and handoff notes.
    - Initialize `Current_Phase_Index = 0`.
EOL
