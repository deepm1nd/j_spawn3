---
id: Trait_CreateResearchReport_FinalizationAndOutputLogic
name: "CreateResearchReport - Finalization and Output Logic"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 6: Finalization and Output"
description: "Defines file output. For SME mode, writes/appends expert content (L0 or L1+) and level-specific references. For Report mode, saves the assembled report. Reports final status."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "file output", "sme output", "report output", "finalization"]
related_traits: ["Trait_CreateResearchReport_ContentAssemblyLogic", "Trait_CreateResearchReport_ValidateParamsAndConstructPaths"]
---
**Full Directive Text:**
BEGIN_PHASE_6
  INFO "Starting Phase 6: Finalization and Output."
  LOAD_STATE_FROM_MEMORY "Phase5_State"
  GET_INTERNAL_VARIABLE "Internal_OPERATIONAL_MODE" store_as "OpMode"
  GET_INTERNAL_VARIABLE "Internal_REQUESTED_RESEARCH_LEVEL" store_as "CurrentResearchLevel"
  # ... (other variables like ResearchSubject, SmeDomainPath, LevelSpecificRefsPath, OutputPath, FileName loaded) ...

  IF_CONDITION "[OpMode]" == "SME_AUGMENTATION" THEN # Corrected
    # ... (Ensure SmeDomainPath exists) ...
    IF_CONDITION "[CurrentResearchLevel]" == "0" THEN
      # ... (Construct L0 expert content for expert_L0.md) ...
      WRITE_FILE "[SmeExpertPath]" from_content "ExpertFileContent"
    ELSE # L1+
      # ... (Append L1+ expert content to expert_L[N].md) ...
      APPEND_FILE "[SmeExpertPath]" from_content "ExpertFileContent" # Assuming SmeExpertPath is expert_L[N].md for L1+
    END_IF
    # Unified Reference File Handling for all levels (L0-L3) using LevelSpecificRefsPath
    IF_CONDITION "[CurrentResearchLevel]" == "0" THEN
        GET_FROM_SYSTEM_MEMORY "Level0_Key_Resources" store_as "RefsListToWrite"
    ELSE
        GET_FROM_SYSTEM_MEMORY "FinalizedOutputReferences" store_as "RefsListToWrite"
    END_IF
    IF_CONDITION "[COUNT_LIST_ITEMS [RefsListToWrite]]" > "0" THEN
      # ... (Construct refs content for refs_L[N].md) ...
      WRITE_FILE "[LevelSpecificRefsPath]" from_content "LevelSpecificRefsContent"
    END_IF
  ELSE_IF_CONDITION "[OpMode]" == "REPORT_GENERATION" THEN # Corrected
    INFO "Operational Mode: Report Generation. Saving report."
    # ... (Get OutputPath, FileName, FinalReportContent) ...
    CREATE_DIRECTORY "[OutputPath]"
    WRITE_FILE "[OutputPath]/[FileName]" with_content "[FinalReportContent]"
    INFO "Report successfully saved to: [OutputPath]/[FileName]"
  ELSE
    FAIL_PROCESS "Invalid Internal_OPERATIONAL_MODE: [OpMode] in Phase 6."
  END_IF
  # Status Reporting (Level 0 specific outputs, etc.)
  # ...
  INFO "Phase 6: Finalization and Output COMPLETED."
  INFO "create_research_report process finished for the requested level."
END_PHASE_6

**Implementation Notes:**
- For "SME" mode:
    - Ensures `SmeDomainPath` directory exists.
    - For Level 0: Creates/overwrites `expert_L0.md` with L0 main summary and subtopic details.
    - For Level > 0: Appends aggregated L[N] content to `expert_L[N].md`.
    - Writes `refs_L[N].md` using `LevelSpecificRefsPath` (populated with `Level0_Key_Resources` for L0, or `FinalizedOutputReferences` for L1+).
- For "Report" mode:
    - Ensures `OutputPath` directory exists.
    - Writes `AssembledReportContent` (from Phase 5) to `[OutputPath]/[FileName]`.
- Reports final status, potentially setting specific output variables for L0 completion.
- `OpMode` should be `SME` or `Report` (correcting typos from source).
EOL
