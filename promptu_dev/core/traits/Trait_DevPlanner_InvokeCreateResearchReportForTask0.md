---
id: Trait_DevPlanner_InvokeCreateResearchReportForTask0
name: "DevPlanner - Invoke create_research_report for Task 0 (Initial Research)"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "II.0.f" # Specifically the main part of f, not the workflow selection
description: "Defines how dev_planner MUST construct a [[USER_CONFIG_FOR_create_research_report]] block to invoke the create_research_report addon for its own 'Task 0' initial research."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "research", "task 0", "create_research_report", "component interaction", "configuration"]
related_traits: ["Trait_DevPlanner_InitialResearchAndKBSetup"]
---
**Full Directive Text:**
f.  Instructions for `dev_planner` when preparing its own "Task 0" (initial research task) which invokes `create_research_report`:
    i.  You MUST construct a `[[USER_CONFIG_FOR_create_research_report]]` block. This block will set the following parameters for the `create_research_report` addon:
        *   `RESEARCH_TOPIC_OR_QUESTIONS`: Synthesize this from a combination of `User_High_Level_Project_Goal` and any items in `Extracted_Input_Concepts_Topics` (from step II.0.c). Ensure the topics are comprehensive and cover all provided inputs and align with the reviewed project specifications.
        *   `PRIMARY_SPECIFICATION_DOCS`: Set using the list of paths/URLs from `Resolved_DP_Project_Specifications_Ref`. (Ensure this is passed as a list or correctly formatted comma-separated string if `create_research_report` expects that).
        *   `GUIDELINES_DOC_PATHS`: Set using the list of paths/URLs from `Resolved_DP_Plan_Methodology_Guides`. (Ensure this is passed as a list or correctly formatted comma-separated string if `create_research_report` expects that).
        *   `EXPERT_CONTENT_PATH`: Set to `../../aidentity/kb/[Resolved_DP_Project_Name]/phase0_initial_research/`
        *   `EXPERT_FILENAME_OVERRIDE`: Set to a descriptive name like `"[Resolved_DP_Project_Name]_initial_research_expert.md"`
        *   `REFERENCE_LIST_PATH`: Set to `../../aidentity/kb/[Resolved_DP_Project_Name]/phase0_initial_research/`
        *   `REFERENCE_FILENAME_OVERRIDE`: Set to a descriptive name like `"[Resolved_DP_Project_Name]_initial_research_refs.md"`
        *   `RESEARCH_REPORT_OUTPUT_PATH`: Set to `../../aidentity/kb/[Resolved_DP_Project_Name]/phase0_initial_research/` (to store the report alongside its KB outputs).
        *   `RESEARCH_REPORT_FILENAME`: Set to a descriptive name like `"[Resolved_DP_Project_Name]_initial_research_report.md"`
        *   `MASTER_DOMAIN_LOG_PATH`: Set to `../../aidentity/kb/master_domain_log.md`
        *   `DOMAIN_NAME_FOR_LOG`: Set to a unique name like `"[Resolved_DP_Project_Name]_InitialResearch"`
        *   `ITERATIVE_OUTPUT_DEPTH`: [USER_VALUE_START] 2 [USER_VALUE_END]

**Implementation Notes:**
- This rule details the precise parameter configuration `dev_planner` must use when it calls `create_research_report` for its initial "Task 0" research.
- `RESEARCH_TOPIC_OR_QUESTIONS` is synthesized from `User_High_Level_Project_Goal` and `Extracted_Input_Concepts_Topics`.
- Other parameters map `dev_planner`'s resolved paths and project details to `create_research_report`'s expected inputs for KB output and logging.
- Ensures research outputs are stored in a project-specific KB subdirectory.
EOL
