---
id: Trait_CreateResearchReport_InputDocumentProcessing
name: "CreateResearchReport - Input Document Processing"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 2: Input Document Processing"
description: "Defines the processing for various input documents: Primary Input, General Input Docs, Exemplars, Guidelines, Additional Web Links File, and Input Reference Files. Involves analysis, summarization, and parsing."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "input processing", "document analysis", "references"]
related_traits: []
---
**Full Directive Text:**
# Phase 2: Input Document Processing
  INFO "Starting Phase 2: Input Document Processing."
  LOAD_STATE_FROM_MEMORY "Phase1_State"

  CREATE_DIRECTORY "[Internal_REPORT_OUTPUT_PATH]/working_temp" store_as "WorkDir" # Assuming Internal_REPORT_OUTPUT_PATH is set for Report mode, or a general temp path.

  INFO "Processing Primary Input Document: [Internal_PRIMARY_INPUT_DOC_PATH]"
  AI_TASK_SUB_PROCESS "Analyze Primary Document"
    # ... (analyze, summarize, store in WorkDir)
  END_AI_TASK_SUB_PROCESS

  INFO "Processing General Input Documents: [Internal_GENERAL_INPUT_DOCS]"
  AI_TASK_REPEATING_SUB_PROCESS "Analyze General Documents"
    # ... (loop, analyze each, store in WorkDir)
  END_AI_TASK_REPEATING_SUB_PROCESS

  IF_CONDITION "[Internal_EXEMPLAR_DOCS]" != "[No Exemplar Documents Provided]" THEN
    INFO "Processing Exemplar Documents: [Internal_EXEMPLAR_DOCS]"
    # ... (similar loop, focus on style/structure)
  END_IF

  IF_CONDITION "[Internal_GUIDELINES_DOC_PATHS]" != "[No Guidelines Provided]" THEN
    INFO "Processing Guidelines Documents: [Internal_GUIDELINES_DOC_PATHS]"
    # ... (similar loop, focus on explicit instructions)
  END_IF

  IF_CONDITION "[Internal_ADDITIONAL_WEB_LINKS_FILE]" != "[No Additional Web Links File Provided]" THEN
    INFO "Processing Additional Web Links File: [Internal_ADDITIONAL_WEB_LINKS_FILE]"
    READ_FILE "[Internal_ADDITIONAL_WEB_LINKS_FILE]" # ... parse and store links
  END_IF

  IF_CONDITION "[Internal_INPUT_REFERENCE_FILES]" != "[Path(s) to input reference files or N/A]" AND "[Internal_INPUT_REFERENCE_FILES]" != "" THEN
    INFO "Processing Input Reference Files: [Internal_INPUT_REFERENCE_FILES]"
    # ... (loop, parse each reference file, store structured refs in LoadedReferencesList)
    # ... (filter into PrioritizedReferencesList if specific identifiers given)
  ELSE
    # ... (create empty PrioritizedReferencesList and LoadedReferencesList)
  END_IF
  # ... (Logic for loading previous iteration data is separated into its own trait)

**Implementation Notes:**
- Creates a working directory for temporary processed files.
- Processes different categories of input documents specified by parameters:
    - Primary Input Document: Analyzed and summarized.
    - General Input Documents: Iteratively analyzed.
    - Exemplar Documents: Processed for style/structure.
    - Guidelines Documents: Processed for explicit instructions.
    - Additional Web Links File: Parsed for more URLs.
    - Input Reference Files: Parsed to extract structured references (ID/Tag, URL, Description) into `LoadedReferencesList`.
    - If `SPECIFIC_REFERENCE_IDENTIFIERS` are provided, `LoadedReferencesList` is filtered into `PrioritizedReferencesList`.
- Results of analysis (summaries, extracted guidelines, styles, references) are stored in system memory.
EOL
