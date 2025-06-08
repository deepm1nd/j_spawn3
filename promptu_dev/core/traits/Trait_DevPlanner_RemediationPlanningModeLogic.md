---
id: Trait_DevPlanner_RemediationPlanningModeLogic
name: "DevPlanner - Remediation Planning Mode Logic"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "VI"
description: "Defines dev_planner's logic when invoked in 'Remediation Planning Mode'. Analyzes rejected/incomplete items from an Acceptance Analysis Report and generates a focused Remediation Task Launch Plan with corrective tasks."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "operational mode", "remediation planning", "acceptance analysis", "task generation", "correction"]
related_traits: ["Trait_DevPlanner_AcceptanceAnalysisModeLogic", "Trait_AIdentityQB_PhasedDevPhase1ToN"]
---
**Full Directive Text:**
**VI. REMEDIATION PLANNING MODE**

A.  **Trigger:** Invoked by an orchestrator (e.g., `PromptuDev_AI`) with a specific goal like "Create Phase Remediation Plan for Phase X based on Acceptance Analysis Report."
B.  **Inputs (Expected from Orchestrator/`handoff_notes.md`):**
    1.  `Current_Phase_ID` (for the phase requiring remediation).
    2.  The "Acceptance Analysis Report" (generated in Section V) detailing rejected/incomplete items.
    3.  Access to the original phase plan and `Resolved_DP_Project_Specifications_Ref`.
C.  **Logic:**
    1.  For each "Rejected" or "Incomplete" item from the Acceptance Analysis Report:
        a.  Analyze the reasons for rejection/incompleteness.
        b.  Define a set of focused, corrective tasks needed to address the deficiencies. These tasks should be granular enough for clear execution.
        c.  These corrective tasks might involve debugging existing code, creating missing documentation, re-implementing a feature to meet specifications, etc.
    2.  These new corrective tasks essentially form a new, scoped-down "mini-plan" or addendum to the original phase plan.
D.  **Output: "Remediation Task Launch Plan"**
    1.  Generate a new Task Launch Plan (TLP) document (e.g., `[Current_Phase_ID]_remediation_tlp.md`) in `Resolved_DP_Task_Prompts_Subdir`.
    2.  This TLP should list:
        *   A clear reference to the `Current_Phase_ID` being remediated.
        *   A summary of the items from the Acceptance Analysis Report that this remediation plan addresses.
        *   A new set of task prompts (following structure in Section III.B) for each corrective task identified in VI.C.1. These prompts should be clearly marked as "Remediation Task for [Original Task ID/Objective]".
    3.  This Remediation TLP is then passed back to the orchestrator to re-enter the user execution and subsequent acceptance analysis cycle for these specific remediation tasks. The goal is to bring the phase to full acceptance.

**Implementation Notes:**
- Triggered by an orchestrator based on a previous Acceptance Analysis Report.
- Inputs: Phase ID, Acceptance Analysis Report, original plan, project specs.
- Logic:
    - For each rejected/incomplete item, define focused corrective tasks.
- Output: A "Remediation Task Launch Plan" containing:
    - Reference to phase and items being addressed.
    - New task prompts for corrective actions (marked as "Remediation Task").
- This TLP is used by the orchestrator to manage the remediation cycle.
EOL
