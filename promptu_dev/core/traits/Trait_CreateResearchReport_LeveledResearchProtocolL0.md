---
id: Trait_CreateResearchReport_LeveledResearchProtocolL0
name: "CreateResearchReport - Leveled Research Protocol (Level 0)"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 3: Level 0 Execution"
description: "Defines Level 0 research: generate detailed overview of main subject, identify 3-7 key subtopics/facets, provide initial detailed explanation and 1-2 key resources for each subtopic, and compile all key resources. Output signals LEVEL_0_COMPLETE."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "research protocol", "level 0 research", "overview", "subtopic identification"]
related_traits: ["Trait_CreateResearchReport_LeveledResearchProtocolLGreaterThan0"]
---
**Full Directive Text:**
# Phase 3: Research and Content Generation (Enhanced for Leveled Protocol)
  IF_CONDITION "[ResearchLevel]" == "0" THEN
    # --- Level 0 Execution ---
    INFO "Executing Level 0 research for subject: [GET_INTERNAL_VARIABLE ResearchSubject]"
    AI_TASK_SUB_PROCESS "Perform Level 0 Research"
      DESCRIPTION "Perform comprehensive L0 research. This includes: generating a detailed overview summary of the main subject; identifying key sub-topics/facets; for each sub-topic, providing an initial detailed explanation and citing 1-2 key resources; and compiling a list of all key resources found. The depth should be comparable to an initial detailed exploration (e.g., a first-pass L1 report)."
      INPUT_PARAM "Subject" from "ResearchSubject"
      INPUT_PARAM "AllReferences" from_list "AllLoadedRefs" (optional)
      INPUT_PARAM "PrioritizedReferences" from_list "PrioritizedRefs" (optional)
      # This sub-process should:
      # a. Research the main `[Subject]` to produce a detailed overview. This summary should explain core concepts, importance, and primary aspects, aiming for detail similar to an introductory chapter or detailed encyclopedia entry. Synthesize from multiple reputable sources.
      # b. Based on main subject research, identify a comprehensive list of 3-7 key sub-topics/facets.
      # c. For **each** identified sub-topic: Perform focused research to provide an initial detailed explanation (defining it, core principles, relation to main subject/other subtopics) and identify 1-2 key specific resources.
      # d. Compile a consolidated list of all key overview resources (main subject) and specific resources (subtopics).
      OUTPUT_TO_SYSTEM_MEMORY "Level0_MainSubject_DetailedSummary" # String
      OUTPUT_TO_SYSTEM_MEMORY "Level0_Identified_Subtopics" # List of strings
      OUTPUT_TO_SYSTEM_MEMORY "Level0_Subtopic_Details_List" # List of Maps: [{subtopic_name: "...", detailed_explanation: "...", specific_resources_list: ["url1", "url2"]}, ...]
      OUTPUT_TO_SYSTEM_MEMORY "Level0_Key_Resources" # List of unique URLs/references - now more comprehensive
    END_AI_TASK_SUB_PROCESS

    INFO "Level 0 research complete. Preparing to pause and report."
    STORE_IN_SYSTEM_MEMORY "ResearchCurrentTopic" from "ResearchSubject"
    SET_INTERNAL_VARIABLE "CurrentResearchStatus" to "LEVEL_0_COMPLETE"
    STORE_IN_SYSTEM_MEMORY "CurrentResearchStatus" from "CurrentResearchStatus"
    INFO "Phase 3: Level 0 Research COMPLETED for this invocation. Status: LEVEL_0_COMPLETE"
  # ... (Else if ResearchLevel > 0)

**Implementation Notes:**
- This is for `REQUESTED_RESEARCH_LEVEL == 0`.
- A sub-process "Perform Level 0 Research" is executed.
- Key outputs of L0 research:
    - `Level0_MainSubject_DetailedSummary`: Detailed overview of the main research subject.
    - `Level0_Identified_Subtopics`: List of 3-7 key subtopics identified.
    - `Level0_Subtopic_Details_List`: For each subtopic, its name, a detailed explanation, and 1-2 specific resources.
    - `Level0_Key_Resources`: Consolidated list of all key resources found.
- After L0 research, `CurrentResearchStatus` is set to "LEVEL_0_COMPLETE".
- The component effectively pauses or ends its run here for L0, expecting the orchestrator to handle L0 outputs.
EOL
