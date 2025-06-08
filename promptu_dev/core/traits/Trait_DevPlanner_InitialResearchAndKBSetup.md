---
id: Trait_DevPlanner_InitialResearchAndKBSetup
name: "DevPlanner - Initial Research and KB Setup"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "II.0.B, II.0.C, II.0.E"
description: "Defines dev_planner's process for setting up KB paths (Mode KB Root, Project KB, Main Research KB, Master Domain Log), processing Planning Input Concepts, and performing a preliminary review of Project Specifications before invoking create_research_report."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "research", "knowledge base", "initialization", "input processing", "specification review"]
related_traits: ["Trait_DevPlanner_InvokeCreateResearchReportForTask0", "Trait_DevPlanner_ConflictAndAmbiguityResolutionProtocol"]
---
**Full Directive Text:**
**B. Initial Research Phase and Knowledge Base Setup (Corresponds to original II.0):**
    a.  Determine `Resolved_DP_Mode_KB_Root` based on `Is_Developer_Session` (e.g., if true, `promptu_dev/kb/`, else `promptu/kb/`).
    b.  Construct the following paths:
        *   `Resolved_DP_Project_KB_Path = [Resolved_DP_Mode_KB_Root][Resolved_DP_Project_Name]/`
        *   `Resolved_DP_Main_Research_KB_SubPath = [Resolved_DP_Project_KB_Path]phase0_initial_research/`
        *   `Resolved_DP_Master_Domain_Log_Path = [Resolved_DP_Mode_KB_Root]master_domain_log.md` (Note: Using a global log for now, can be project-specific later if needed).
    c.  **Process Planning Input Concepts (if provided):**
        1.  Initialize an internal variable `Extracted_Input_Concepts_Topics` as an empty list or collection.
        2.  Check if `Resolved_DP_Planning_Input_Concepts_Dir` is provided (not N/A or empty) and points to an existing directory.
        3.  If yes:
            a.  List all files within `Resolved_DP_Planning_Input_Concepts_Dir`.
            b.  For each file:
                i.  Perform a conceptual analysis (e.g., summarization, keyword/topic extraction) to identify key concepts, questions, or main themes.
                ii. Append these extracted items to `Extracted_Input_Concepts_Topics`.
    e.  **Preliminary Review of Project Specifications:**
        1.  Before configuring the research task, perform a preliminary review of the documents/URLs provided in `Resolved_DP_Project_Specifications_Ref` (if any).
        2.  The purpose is to identify any obvious, high-level contradictions or critical ambiguities that would significantly hinder the initial research phase by making its scope unclear or its goals unachievable.
    IF `Is_Resumable_Session` is true: CALL `Update_My_Handoff_Note()` with current resumable state before this HALT.
        3.  If such show-stopping issues are identified (e.g., core requirements that are mutually exclusive, key terms defined inconsistently across essential documents), HALT operations and present these critical conflicts/ambiguities to the user for clarification. State clearly that proceeding with research without resolving these foundational issues would be counterproductive.
        4.  If no such critical preliminary issues are found, or if `Resolved_DP_Project_Specifications_Ref` is not provided, proceed to configure and invoke `create_research_report`.

**Implementation Notes:**
- Determines KB paths based on `Is_Developer_Session` and `Resolved_DP_Project_Name`.
- Processes files in `Resolved_DP_Planning_Input_Concepts_Dir` to extract topics for research.
- Performs a preliminary review of `Resolved_DP_Project_Specifications_Ref` for show-stopping issues.
- If critical issues found, HALTS and reports to user (related to Conflict/Ambiguity protocol but specific to this pre-research phase).
- This setup is prerequisite for invoking `create_research_report` for "Task 0".
- Notes resumable session handoff update point.
EOL
