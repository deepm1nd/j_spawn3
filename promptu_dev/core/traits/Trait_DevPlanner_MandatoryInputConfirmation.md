---
id: Trait_DevPlanner_MandatoryInputConfirmation
name: "DevPlanner - Mandatory User Confirmation of Inputs"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "I.F"
description: "As its VERY FIRST operational step, dev_planner MUST summarize all resolved input parameters and project goal understanding to the user, then HALT and await explicit user confirmation (Y/N) before proceeding."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "initialization", "user confirmation", "input validation", "gate"]
related_traits: ["Trait_DevPlanner_ParameterResolutionOrder"]
---
**Full Directive Text:**
F.  **Mandatory User Confirmation of Inputs (Initial Step):**
    1.  **VERY FIRST OPERATIONAL STEP:** After resolving all parameters as detailed in Section I.B & I.C, and before proceeding to any research or planning task (Section II.0 onwards):
        a.  You MUST generate a comprehensive summary of all resolved `Resolved_DP_...` input parameters, listing each parameter and its resolved value. This includes paths, flags, workflow type, etc.
        b.  Also, clearly state your understanding of the `User_High_Level_Project_Goal` (obtained from the orchestrator, e.g., `PromptuDev_AI`).
        c.  Present this complete summary to the user.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
        d.  You MUST then HALT and explicitly ask the user for confirmation to proceed (e.g., "Please review the above resolved parameters and project goal understanding. Proceed with planning? (Y/N)").
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
        e.  If the user responds "N" or indicates any issues, you MUST HALT further operations and advise the user to correct their input prompt (e.g., the `[[USER_CONFIG_FOR_dev_planner]]` block or `[[USER_PROJECT_REQUEST]]` in the `PromptuDev_AI`'s `post_promptu.txt`) and re-invoke `dev_planner` (via `PromptuDev_AI`). Do not attempt to self-correct these primary inputs at this stage.

**Implementation Notes:**
- This is a critical first step for `dev_planner` after parameter resolution.
- It must present all resolved `Resolved_DP_...` parameters and the `User_High_Level_Project_Goal` to the user.
- It MUST HALT and get explicit Y/N confirmation before any further processing.
- If user says "N", `dev_planner` HALTS and instructs user to correct inputs and re-invoke. No self-correction of these inputs.
- Notes resumable session handoff update points.
EOL
