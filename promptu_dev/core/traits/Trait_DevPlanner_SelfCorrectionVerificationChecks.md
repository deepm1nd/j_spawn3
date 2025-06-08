---
id: Trait_DevPlanner_SelfCorrectionVerificationChecks
name: "DevPlanner - Self-Correction and Verification Checks on Task Prompts"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "IV.A"
description: "Defines mandatory self-correction checks dev_planner MUST perform on each generated task prompt before finalizing. Checks include core directives, path placeholders, add-on inclusion, clarity, and dependencies."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "self-correction", "verification", "task prompt", "quality assurance"]
related_traits: ["Trait_DevPlanner_TaskPromptFileStructure"]
---
**Full Directive Text:**
A. **Planning AI Self-Correction and Verification Checks (MANDATORY):**
    Before finalizing, perform these checks on *each generated task prompt*:
    1. Core Directives: `dev_plan.md`? `next_steps.md` (with `### Key Learnings & Discoveries`)? IEP reminder? Risk Assessment directive? Retry Logic?
    2. Path Placeholders: All paths constructed correctly using `Resolved_DP_...` parameters (from Section I of this add-on)? Specific output subdirectories like `<PhaseTaskName>` correctly appended where necessary? Paths have ", it is in the repo."?
    3. Selected Add-on Inclusion: Logic for `Inheritable_Addons_Content_Ordered_List` (from Core Instructions Section I.D.5) applied? Selected add-ons appended with internal placeholders resolved according to mappings in Section III.B.11.b.i of this add-on?
    4. Clarity: Task Overview & Instructions clear?
    5. Dependency Statement: Coherent if task is dependent?
    Correct any discrepancies.

**Implementation Notes:**
- This is a MANDATORY set of checks `dev_planner` must run on every task prompt it generates.
- Checks cover:
    1. Presence of core directive elements in the prompt (dev_plan, next_steps, IEP, risk assessment, retry).
    2. Correctness of path placeholders (using `Resolved_DP_...` params, correct subdirs, ", it is in the repo." suffix).
    3. Proper inclusion and placeholder resolution for inheritable add-ons.
    4. Clarity of overview and instructions.
    5. Coherence of dependency statements.
- `dev_planner` must correct any found discrepancies.
EOL
