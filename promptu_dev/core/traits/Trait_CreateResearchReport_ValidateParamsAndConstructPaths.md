---
id: Trait_CreateResearchReport_ValidateParamsAndConstructPaths
name: "CreateResearchReport - Validate Output Mode and Construct Paths"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 1: Validation based on OUTPUT_MODE and path construction"
description: "After parameter resolution, validates TARGET_SME_DOMAIN_NAME or REPORT_IDENTIFIER based on OUTPUT_MODE. Constructs paths for SME (domain path, level-specific refs) or Report (output dir, base name) outputs."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "configuration", "validation", "file paths", "output mode", "sme", "report"]
related_traits: ["Trait_CreateResearchReport_ParameterDefinitionAndResolution"]
---
**Full Directive Text:**
# (In Phase 1, after parameter resolution loop)
  # Validate required inputs based on OUTPUT_MODE and construct paths
  IF_CONDITION "[Internal_OUTPUT_MODE]" == "SME" THEN
    INFO "Operational Mode: SME Augmentation"
    IF_CONDITION "[Internal_TARGET_SME_DOMAIN_NAME]" == "[SME Domain Name]" OR "[Internal_TARGET_SME_DOMAIN_NAME]" == "" THEN
      FAIL_PROCESS "TARGET_SME_DOMAIN_NAME is required for SME output mode."
    END_IF
    # Construct SME paths
    SET_INTERNAL_VARIABLE "KbBaseDir" to "promptu_dev/kb"
    SET_INTERNAL_VARIABLE "SmeDomainPath" as_path_join "[KbBaseDir]" "[Internal_TARGET_SME_DOMAIN_NAME]"
    STORE_IN_SYSTEM_MEMORY "Calculated_SME_Domain_Path" from "SmeDomainPath"
    INFO "Target SME Domain Path: [SmeDomainPath]"

    # Refs file path is now level-specific for SME mode
    GET_INTERNAL_VARIABLE "Internal_REQUESTED_RESEARCH_LEVEL" store_as "CurrentResearchLevelForSMEPath"
    SET_INTERNAL_VARIABLE "LevelSpecificSmeRefsFileName" to "refs_L[CurrentResearchLevelForSMEPath].md"
    SET_INTERNAL_VARIABLE "LevelSpecificSmeRefsPath" as_path_join "[SmeDomainPath]" "[LevelSpecificSmeRefsFileName]"
    STORE_IN_SYSTEM_MEMORY "Calculated_SME_Refs_Path" from "LevelSpecificSmeRefsPath"
    INFO "Actual Target SME Refs File (level-specific): [LevelSpecificSmeRefsPath]"

  ELSE_IF_CONDITION "[Internal_OUTPUT_MODE]" == "Report" THEN
    INFO "Operational Mode: Report Generation"
    IF_CONDITION "[Internal_REPORT_IDENTIFIER]" == "[Report Identifier]" OR "[Internal_REPORT_IDENTIFIER]" == "" THEN
      FAIL_PROCESS "REPORT_IDENTIFIER is required for Report output mode."
    END_IF
    # Construct Report paths
    SET_INTERNAL_VARIABLE "ReportBaseDir" to "promptu_dev/reports"
    SET_INTERNAL_VARIABLE "ReportOutputDir" as_path_join "[ReportBaseDir]" "[Internal_REPORT_IDENTIFIER]"
    SET_INTERNAL_VARIABLE "ReportBaseName" to "[Internal_REPORT_IDENTIFIER]_L[Internal_REQUESTED_RESEARCH_LEVEL]"
    STORE_IN_SYSTEM_MEMORY "Calculated_Report_Output_Dir" from "ReportOutputDir"
    STORE_IN_SYSTEM_MEMORY "Calculated_Report_Base_Name" from "ReportBaseName"
    INFO "Target Report Output Directory: [ReportOutputDir]"
    INFO "Target Report Basename: [ReportBaseName]"
  ELSE
    FAIL_PROCESS "Invalid OUTPUT_MODE specified: [Internal_OUTPUT_MODE]. Must be SME or Report."
  END_IF

  # RESEARCH_INPUT validation (already marked as required with NON_EMPTY_STRING_OR_PATH_URL)
  # Validation for PREVIOUS_ITERATION_OUTPUT_PATH if REQUESTED_RESEARCH_LEVEL > 0
  IF_CONDITION "[Internal_REQUESTED_RESEARCH_LEVEL]" > "0" THEN
    IF_CONDITION "[Internal_PREVIOUS_ITERATION_OUTPUT_PATH]" != "[Path to previous iteration state or N/A]" AND "[Internal_PREVIOUS_ITERATION_OUTPUT_PATH]" != "" THEN
        VALIDATE_PARAMETER "[Internal_PREVIOUS_ITERATION_OUTPUT_PATH]" against "PATH_EXISTS" store_as "PrevIterPathVal"
        IF "[PrevIterPathVal]" != "VALID" THEN
            FAIL_PROCESS "PREVIOUS_ITERATION_OUTPUT_PATH must be a valid path if provided and REQUESTED_RESEARCH_LEVEL > 0. Current value: [Internal_PREVIOUS_ITERATION_OUTPUT_PATH]"
        END_IF
        INFO "Previous iteration state path set to: [Internal_PREVIOUS_ITERATION_OUTPUT_PATH]"
    END_IF
  END_IF

**Implementation Notes:**
- Post-parameter resolution, this logic branch is based on `Internal_OUTPUT_MODE`.
- If "SME":
    - Validates `Internal_TARGET_SME_DOMAIN_NAME`.
    - Constructs `SmeDomainPath` (e.g., `promptu_dev/kb/[DomainName]`).
    - Constructs level-specific SME refs path (`Calculated_SME_Refs_Path`) e.g., `.../[DomainName]/refs_L[N].md`.
- If "Report":
    - Validates `Internal_REPORT_IDENTIFIER`.
    - Constructs `ReportOutputDir` and `ReportBaseName` (e.g., `promptu_dev/reports/[Identifier]`, `[Identifier]_L[N]`).
- Fails if `OUTPUT_MODE` is invalid.
- Validates `PREVIOUS_ITERATION_OUTPUT_PATH` if `REQUESTED_RESEARCH_LEVEL > 0`.
EOL
