---
id: Trait_DevPlanner_ConflictAndAmbiguityResolutionProtocol
name: "DevPlanner - Conflict and Ambiguity Resolution Protocol"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "I.E"
description: "If critical conflicts or ambiguities are found in specification documents that prevent decision-making, dev_planner MUST HALT, present the issue and source to the user, request specific clarification, and document it before resuming."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "error handling", "user interaction", "specifications", "conflict resolution", "ambiguity"]
related_traits: []
---
**Full Directive Text:**
E.  **Conflict and Ambiguity Resolution Protocol:**
    Throughout all planning stages, if you (the AI running `dev_planner`) or any Task AI you spawn identifies critical conflicts or ambiguities within or between documents in `Resolved_DP_Project_Specifications_Ref` (or other guiding documents) that prevent confident decision-making:
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
    1.  **HALT** the specific planning or task generation step.
    2.  **Present the Issue Clearly to the User:**
        *   Identify the source documents and specific sections/items that are conflicting or ambiguous.
        *   Clearly articulate the nature of the conflict (e.g., "Requirement A.1 states X, while Requirement B.2 states Y, and these are mutually exclusive for component Z.")
        *   For ambiguities, explain the different potential interpretations and why the ambiguity is blocking (e.g., "Requirement C.3 states 'system must be performant,' which could mean response times under 1s or resource utilization below 50%. Clarification is needed to select appropriate architecture for module W.")
    3.  **Request Specific Clarification:** Ask the user to provide a decision, clarify the requirement, or confirm the correct interpretation.
    4.  **Document and Resume:** Once clarification is received from the user, document this clarification (e.g., in an assumptions log or directly in the evolving plan document). Then, resume the planning process using the user's feedback.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
    5.  Spawned Task AIs that detect such issues relevant to their specific task should typically report them back to `dev_planner` (or the orchestrating AI) or, if severe, directly to the user as per their prompt's error handling instructions, possibly by HALTING their own execution and clearly stating the problem.

**Implementation Notes:**
- This protocol is triggered when `dev_planner` or a spawned Task AI encounters blocking conflicts/ambiguities in `Resolved_DP_Project_Specifications_Ref`.
- Steps:
    1. HALT.
    2. Present issue to user (source, nature of conflict/ambiguity).
    3. Request user clarification.
    4. Document clarification and resume.
- Task AIs can report back to `dev_planner` or halt and report to user.
- Notes resumable session handoff update points.
EOL
