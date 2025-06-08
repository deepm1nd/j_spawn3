---
id: Trait_CreateResearchReport_ReportOutputStructureDefinition
name: "CreateResearchReport - Report/Output Structure Definition"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 4: Report/Output Structure Definition"
description: "Defines the output structure. For SME mode, notes potential sections from L0 subtopics. For Report mode, uses user's ProposedToCInput, or derives ToC from L0 subtopics, primary doc structure, or a fallback ToC."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "report structure", "table of contents", "sme structure", "output definition"]
related_traits: ["Trait_CreateResearchReport_LeveledResearchProtocolL0"]
---
**Full Directive Text:**
BEGIN_PHASE_4
  INFO "Starting Phase 4: Report/Output Structure Definition."
  LOAD_STATE_FROM_MEMORY "Phase3_State"
  GET_INTERNAL_VARIABLE "Internal_OPERATIONAL_MODE" store_as "OpMode"

  IF_CONDITION "[OpMode]" == "SME_AUGMENTATION" THEN # Corrected from SME_AUGMENTATION
    INFO "Operational Mode: SME Augmentation. Report ToC structure is less critical."
    IF_SYSTEM_MEMORY_CONTAINS "Level0_Identified_Subtopics" THEN
      GET_FROM_SYSTEM_MEMORY "Level0_Identified_Subtopics" store_as "SME_Potential_Sections"
      STORE_IN_SYSTEM_MEMORY "SME_Structure_Guidance" from_list "SME_Potential_Sections"
    ELSE
      CREATE_LIST "EmptyGuidance"
      STORE_IN_SYSTEM_MEMORY "SME_Structure_Guidance" from_list "EmptyGuidance"
    END_IF
    CREATE_LIST "SME_Mode_ToC_Placeholder" # Ensure FinalToC is empty for SME for Phase 5
    STORE_IN_SYSTEM_MEMORY "FinalToC" from_list "SME_Mode_ToC_Placeholder"
  ELSE_IF_CONDITION "[OpMode]" == "REPORT_GENERATION" THEN # Corrected from REPORT_GENERATION
    INFO "Operational Mode: Report Generation. Defining report structure."
    GET_INTERNAL_VARIABLE "Internal_PROPOSED_TABLE_OF_CONTENTS" store_as "ProposedToCInput"
    IF_CONDITION "[ProposedToCInput]" == "[No Table of Contents Provided]" THEN
      INFO "No proposed Table of Contents. Attempting to derive..."
      IF_SYSTEM_MEMORY_CONTAINS "Level0_Identified_Subtopics" THEN
        GET_FROM_SYSTEM_MEMORY "Level0_Identified_Subtopics" store_as "DerivedToCList"
        STORE_IN_SYSTEM_MEMORY "FinalToC" from_list "DerivedToCList"
      ELSE_IF_SYSTEM_MEMORY_CONTAINS "AnalyzedPrimaryDoc.Structure" THEN
        GET_FROM_SYSTEM_MEMORY "AnalyzedPrimaryDoc.Structure" store_as "PrimaryDocToCList"
        STORE_IN_SYSTEM_MEMORY "FinalToC" from_list "PrimaryDocToCList"
      ELSE
        GET_INTERNAL_VARIABLE "ResearchSubject" store_as "SubjectForToC"
        CREATE_LIST "FallbackToC" item_1 "Introduction to [SubjectForToC]" item_2 "Key Findings for [SubjectForToC]" item_3 "Conclusion for [SubjectForToC]"
        STORE_IN_SYSTEM_MEMORY "FinalToC" from_list "FallbackToC"
      END_IF
    ELSE
      AI_TASK_SUB_PROCESS "Parse Proposed ToC"
        INPUT_PARAM "ProposedToCInput" from "ProposedToCInput"
        OUTPUT_TO_SYSTEM_MEMORY "ParsedToCList"
      END_AI_TASK_SUB_PROCESS
      GET_FROM_SYSTEM_MEMORY "ParsedToCList" store_as "UserToC"
      STORE_IN_SYSTEM_MEMORY "FinalToC" from_list "UserToC"
    END_IF
    # Optional: AI reviews ToC and content for coherence (omitted in this pass).
  END_IF
  INFO "Phase 4: Report/Output Structure Definition COMPLETED."
  SAVE_CURRENT_STATE_TO_MEMORY "Phase4_State"
END_PHASE_4

**Implementation Notes:**
- For "SME" mode:
    - ToC is less critical.
    - Stores `Level0_Identified_Subtopics` as `SME_Structure_Guidance`.
    - Sets `FinalToC` to an empty list placeholder.
- For "Report" mode:
    - If `Internal_PROPOSED_TABLE_OF_CONTENTS` is provided, it's parsed and used as `FinalToC`.
    - If not, `FinalToC` is derived in order of preference:
        1. `Level0_Identified_Subtopics` (from Phase 3).
        2. `AnalyzedPrimaryDoc.Structure` (from Phase 2).
        3. Fallback ToC based on `ResearchSubject`.
- `OpMode` should be `SME` or `Report` (correcting typos from source).
EOL
