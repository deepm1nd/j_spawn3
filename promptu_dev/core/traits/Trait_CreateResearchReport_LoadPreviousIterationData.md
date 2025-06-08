---
id: Trait_CreateResearchReport_LoadPreviousIterationData
name: "CreateResearchReport - Load Previous Iteration Data for Leveled Research"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 2: If Iterative Depth > 0, load previous iteration's output"
description: "If REQUESTED_RESEARCH_LEVEL > 0, this logic loads and parses the expert_L0.md from PREVIOUS_ITERATION_OUTPUT_PATH to extract subtopics for L1+ research. Optionally loads L0 references."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "leveled research", "iterative research", "input processing", "subtopic extraction"]
related_traits: ["Trait_CreateResearchReport_InputDocumentProcessing", "Trait_CreateResearchReport_LeveledResearchProtocolLGreaterThan0"]
---
**Full Directive Text:**
# (In Phase 2, after other input document processing)
  IF_CONDITION "[Internal_REQUESTED_RESEARCH_LEVEL]" > "0" THEN
    AI_TASK_SUB_PROCESS "Load Previous Iteration Data"
      DESCRIPTION "Load and parse L0 output (expert_L0.md) from the previous iteration if path is provided, to extract subtopics for L1+ research."
      INPUT_PARAM "PreviousOutputPath" from "Internal_PREVIOUS_ITERATION_OUTPUT_PATH"
      INPUT_PARAM "RequestedLevel" from "Internal_REQUESTED_RESEARCH_LEVEL"
      OUTPUT_TO_SYSTEM_MEMORY "UserSpecifiedSubtopicsForL[RequestedLevel]"
      OUTPUT_TO_SYSTEM_MEMORY "LoadedL0References" (optional)
      # Internal Logic for "Load Previous Iteration Data":
      # 1.
      INFO "Attempting to load L0 data from [PreviousOutputPath] for L[RequestedLevel] research."
      # 2.
      SET_INTERNAL_VARIABLE "PathToL0Expert" as_path_join "[PreviousOutputPath]" "expert_L0.md"
      # 3.
      IF_FILE_EXISTS "[PathToL0Expert]" THEN
        # a.
        READ_FILE "[PathToL0Expert]" store_as "L0ExpertContent"
        # b.
        AI_TASK_SUB_PROCESS "Parse L0 Expert Content for Subtopics"
          # i.
          DESCRIPTION "Extract subtopics from L0 expert markdown."
          # ii.
          INPUT_PARAM "MarkdownContent" from "L0ExpertContent"
          # iii. This sub-process needs to parse the "## Identified Subtopics/Facets" section and extract the list items.
          # iv.
          OUTPUT_TO_SYSTEM_MEMORY "ParsedL0SubtopicsList"
        END_AI_TASK_SUB_PROCESS
        # c.
        GET_FROM_SYSTEM_MEMORY "ParsedL0SubtopicsList" store_as "ExtractedSubtopics"
        # d.
        IF_CONDITION "[COUNT_LIST_ITEMS [ExtractedSubtopics]]" > "0" THEN
          # i.
          STORE_IN_SYSTEM_MEMORY "UserSpecifiedSubtopicsForL[RequestedLevel]" from_list "ExtractedSubtopics"
          # ii.
          INFO "Successfully loaded [COUNT_LIST_ITEMS [ExtractedSubtopics]] subtopics from [PathToL0Expert] for L[RequestedLevel] research."
        # e.
        ELSE
          # i.
          INFO "No subtopics found or extracted from [PathToL0Expert]."
        # f.
        END_IF
      # 4.
      ELSE
        # a.
        INFO "No `expert_L0.md` found at [PathToL0Expert]. Proceeding without L0 subtopics."
      # 5.
      END_IF
      # 6. (Optional: Similar logic to load `refs_L0.md` and store its content in `LoadedL0References` if needed by Phase 3 for L1+).
    END_AI_TASK_SUB_PROCESS
  END_IF

**Implementation Notes:**
- This logic executes if `Internal_REQUESTED_RESEARCH_LEVEL` is greater than 0.
- It attempts to load `expert_L0.md` from the `Internal_PREVIOUS_ITERATION_OUTPUT_PATH`.
- If the L0 file exists, it's read, and a sub-process parses it to extract subtopics (typically from a "## Identified Subtopics/Facets" section).
- These extracted subtopics are stored in `UserSpecifiedSubtopicsForL[RequestedLevel]` for use in Phase 3's L1+ research.
- Optionally, `refs_L0.md` could also be loaded.
EOL
