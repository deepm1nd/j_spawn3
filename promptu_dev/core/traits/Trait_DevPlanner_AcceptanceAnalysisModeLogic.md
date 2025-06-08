---
id: Trait_DevPlanner_AcceptanceAnalysisModeLogic
name: "DevPlanner - Acceptance Analysis Mode Logic"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "V"
description: "Defines dev_planner's logic when invoked in 'Acceptance Analysis Mode'. Compares actual deliverables against planned items and specs, determines status (Accepted/Rejected/Incomplete), and generates an Acceptance Analysis Report."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "operational mode", "acceptance analysis", "deliverables", "verification", "reporting"]
related_traits: ["Trait_AIdentityQB_PhasedDevPhase1ToN"] # Orchestrator uses this
---
**Full Directive Text:**
**V. ACCEPTANCE ANALYSIS MODE**

A.  **Trigger:** Invoked by an orchestrator (e.g., `PromptuDev_AI`) with a specific goal like "Perform Phase Acceptance Analysis for Phase X."
B.  **Inputs (Expected from Orchestrator/`handoff_notes.md`):**
    1.  `Current_Phase_ID` (or similar identifier for the phase being analyzed).
    2.  A clear list of, or paths to, the actual merged deliverables produced by the user/developer Task AIs for this phase.
    3.  The original detailed plan for `Current_Phase_ID` (e.g., the `[PhaseID]_submodule_dev_plan.md` or the main plan if monolithic for that phase) which contains the objectives, tasks, and acceptance criteria.
    4.  Access to `Resolved_DP_Project_Specifications_Ref` for cross-referencing.
C.  **Logic:**
    1.  For each task/objective/deliverable defined in the original phase plan:
        a.  Compare the actual deliverable(s) against the planned item and its acceptance criteria.
        b.  Verify adherence to relevant sections of `Resolved_DP_Project_Specifications_Ref`.
        c.  Determine status: "Accepted," "Rejected," or "Incomplete."
D.  **Output: "Acceptance Analysis Report"**
    1.  Generate a new Markdown document (e.g., `[Current_Phase_ID]_acceptance_analysis_report.md`) in `Resolved_DP_Submodule_Plans_Subdir`.
    2.  **Structure:**
        *   **Overall Phase Summary:** Brief statement of acceptance status.
        *   **Detailed Item Breakdown:** For each planned task/deliverable:
            *   Planned Item, Deliverable(s) Provided, Status, Analysis & Justification, Recommendations.
    3.  This report is then passed back to the orchestrator.

**Implementation Notes:**
- This mode is triggered by an orchestrator for a specific phase.
- Requires inputs: Phase ID, paths to deliverables, original phase plan, project specs.
- Logic:
    - Compares deliverables to planned items/acceptance criteria.
    - Verifies against project specifications.
    - Assigns status: Accepted, Rejected, Incomplete.
- Output: A structured "Acceptance Analysis Report" detailing findings for each item.
- The report is then used by the orchestrator (e.g., `PromptuDev_AI` in `Trait_AIdentityQB_PhasedDevPhase1ToN`).
EOL
