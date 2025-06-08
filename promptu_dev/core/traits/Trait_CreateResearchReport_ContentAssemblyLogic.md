---
id: Trait_CreateResearchReport_ContentAssemblyLogic
name: "CreateResearchReport - Content Assembly Logic"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 5: Content Assembly"
description: "Defines content assembly. For SME mode, prepares aggregated expert content and references. For Report mode, assembles report using FinalToC, mapping content to headings (with fallback to general aggregated content) and appending references."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "content assembly", "report generation", "sme content"]
related_traits: ["Trait_CreateResearchReport_ReportOutputStructureDefinition", "Trait_CreateResearchReport_LeveledResearchProtocolLGreaterThan0"]
---
**Full Directive Text:**
BEGIN_PHASE_5
  INFO "Starting Phase 5: Content Assembly."
  LOAD_STATE_FROM_MEMORY "Phase4_State"
  GET_INTERNAL_VARIABLE "Internal_OPERATIONAL_MODE" store_as "OpMode"
  # ... (other variables like BaseName, ResearchLevel loaded)

  IF_CONDITION "[OpMode]" == "SME_AUGMENTATION" THEN # Corrected from SME_AUGMENTATION
    INFO "Operational Mode: SME Augmentation. Preparing content for SME files."
    GET_FROM_SYSTEM_MEMORY "FinalizedOutputContent" store_as "SME_Expert_Content_For_Append"
    STORE_IN_SYSTEM_MEMORY "SME_Expert_Content_For_Append" from "SME_Expert_Content_For_Append"
    GET_FROM_SYSTEM_MEMORY "FinalizedOutputReferences" store_as "SME_Refs_List_For_Append"
    STORE_IN_SYSTEM_MEMORY "SME_Refs_List_For_Append" from_list "SME_Refs_List_For_Append"
  ELSE_IF_CONDITION "[OpMode]" == "REPORT_GENERATION" THEN # Corrected from REPORT_GENERATION
    INFO "Operational Mode: Report Generation. Assembling report."
    GET_FROM_SYSTEM_MEMORY "FinalToC" store_as "ReportToC"
    # ... (ReportFileName constructed) ...
    CREATE_FILE_CONTENT "AssembledReport"
    APPEND_TO_CONTENT "AssembledReport" "# Research Report: [BaseName] - Level [ResearchLevel]\n\n"
    # ... (Primary Subject added) ...

    IF_CONDITION "[COUNT_LIST_ITEMS [ReportToC]]" > "0" THEN
        APPEND_TO_CONTENT "AssembledReport" "## Table of Contents\n"
        # ... (Loop to create ToC with anchor links) ...
        APPEND_TO_CONTENT "AssembledReport" "\n---\n\n"
        LOOP_THROUGH_LIST "ReportToC" as "HeadingItem"
            APPEND_TO_CONTENT "AssembledReport" "## [HeadingItem]\n\n"
            GET_FROM_SYSTEM_MEMORY "GeneratedContent_[HeadingItem]" store_as "SectionContent" (default_value "") # Attempt specific content
            IF_CONDITION "[SectionContent]" == "" THEN
                GET_FROM_SYSTEM_MEMORY "FinalizedOutputContent" store_as "SectionContent" # Fallback to aggregated
                APPEND_TO_CONTENT "AssembledReport" "[SectionContent]\n\n"
                IF_CONDITION "[ITEM_INDEX]" == "0" THEN STORE_IN_SYSTEM_MEMORY "FinalizedOutputContent" from "" # Use aggregated once
            ELSE
                 APPEND_TO_CONTENT "AssembledReport" "[SectionContent]\n\n"
            END_IF
        END_LOOP
    ELSE
        INFO "No Table of Contents defined. Appending all generated content."
        GET_FROM_SYSTEM_MEMORY "FinalizedOutputContent" store_as "MainReportContent"
        APPEND_TO_CONTENT "AssembledReport" "[MainReportContent]\n\n"
    END_IF
    # ... (Handle bibliography/references) ...
    STORE_IN_SYSTEM_MEMORY "AssembledReportContent" from_content "AssembledReport"
  ELSE
    FAIL_PROCESS "Invalid Internal_OPERATIONAL_MODE: [OpMode] in Phase 5."
  END_IF
  INFO "Phase 5: Content Assembly COMPLETED."
  SAVE_CURRENT_STATE_TO_MEMORY "Phase5_State"
END_PHASE_5

**Implementation Notes:**
- For "SME" mode:
    - Takes `FinalizedOutputContent` (aggregated from Phase 3) as `SME_Expert_Content_For_Append`.
    - Takes `FinalizedOutputReferences` (aggregated from Phase 3) as `SME_Refs_List_For_Append`.
- For "Report" mode:
    - Creates `AssembledReport` content in memory.
    - Adds title and primary subject.
    - If `FinalToC` exists:
        - Generates a ToC section with anchor links.
        - Loops through `FinalToC` headings:
            - Appends heading.
            - Tries to get specific content for that heading (e.g., `GeneratedContent_[HeadingItem]`).
            - If specific content not found, falls back to using `FinalizedOutputContent` (aggregated from Phase 3), attempting to use it only once for the first such section.
    - If no `FinalToC`, appends all `FinalizedOutputContent`.
    - Appends formatted references from `FinalizedOutputReferences`.
    - Stores the complete report in `AssembledReportContent`.
- `OpMode` should be `SME` or `Report` (correcting typos from source).
EOL
