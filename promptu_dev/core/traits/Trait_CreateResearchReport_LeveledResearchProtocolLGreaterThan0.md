---
id: Trait_CreateResearchReport_LeveledResearchProtocolLGreaterThan0
name: "CreateResearchReport - Leveled Research Protocol (Level > 0)"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 3: Level > 0 Execution"
description: "Defines L1+ research: iterate through subtopics (from L0 or main subject if none), perform deep dive research for each based on level-specific instructions (L1, L2, L3 aim for ~4x prior level's content), and aggregate content and references."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "research protocol", "leveled research", "deep dive", "subtopic research"]
related_traits: ["Trait_CreateResearchReport_LeveledResearchProtocolL0", "Trait_CreateResearchReport_LoadPreviousIterationData"]
---
**Full Directive Text:**
# Phase 3: Research and Content Generation (Enhanced for Leveled Protocol)
  # ... (If ResearchLevel == 0 logic) ...
  ELSE_IF_CONDITION "[ResearchLevel]" > "0" THEN
    # --- Level > 0 Execution (Deeper Dive) ---
    INFO "Executing Level [ResearchLevel] research for subject: [GET_INTERNAL_VARIABLE ResearchSubject]"
    CREATE_STRING "AggregatedContent_Level[ResearchLevel]"
    CREATE_LIST "AggregatedReferences_Level[ResearchLevel]"
    GET_FROM_SYSTEM_MEMORY "UserSpecifiedSubtopicsForL[ResearchLevel]" store_as "CurrentLevelSubtopics" (default_value_is_empty_list)

    IF_CONDITION "[COUNT_LIST_ITEMS [CurrentLevelSubtopics]]" == "0" THEN
      INFO "No subtopics specified for L[ResearchLevel]. Researching main subject: [ResearchSubject]"
      ADD_ITEM_TO_LIST "CurrentLevelSubtopics" item "[ResearchSubject]"
    END_IF

    AI_TASK_REPEATING_SUB_PROCESS "Iterate Research Over Subtopics for Level [ResearchLevel]"
      INITIAL_STATE_FROM_LIST "CurrentLevelSubtopics"
      ACTION_PER_ITEM "Research Subtopic: [ITEM_VALUE] (Index: [ITEM_INDEX])"
        SET_INTERNAL_VARIABLE "CurrentSubtopic" to "[ITEM_VALUE]"
        SET_INTERNAL_VARIABLE "Current_Subtopic_Name" to "[ITEM_VALUE]" # For specific depth instructions
        SET_INTERNAL_VARIABLE "TargetContentMultiplier" to "4"
        # Level-specific instructions (L1, L2, L3 aim for ~4x prior level's content, specific focus for each)
        IF_CONDITION "[ResearchLevel]" == "1" THEN SET_INTERNAL_VARIABLE "SpecificDepthInstructions" to "For this L1 dive... Aim for ~[TargetContentMultiplier]x content of an L0 overview..."
        ELSE_IF_CONDITION "[ResearchLevel]" == "2" THEN SET_INTERNAL_VARIABLE "SpecificDepthInstructions" to "For this L2 dive... Aim for ~[TargetContentMultiplier]x content of L1..."
        ELSE_IF_CONDITION "[ResearchLevel]" == "3" THEN SET_INTERNAL_VARIABLE "SpecificDepthInstructions" to "For this L3 (Ludicrous depth) dive... Aim for ~[TargetContentMultiplier]x content of L2..."
        ELSE SET_INTERNAL_VARIABLE "SpecificDepthInstructions" to "Perform a general deep dive..."
        END_IF

        AI_TASK_SUB_PROCESS "Perform Deep Dive Research (L[ResearchLevel] - Subtopic: [CurrentSubtopic])"
          DESCRIPTION "Perform deeper research on a specific subtopic, following specific depth instructions."
          # Inputs: Subject (CurrentSubtopic), MainResearchSubject, RequestedLevel, ReferenceFollowingDepth, SpecificDepthInstructions, AllReferences, PrioritizedReferences
          OUTPUT_TO_SYSTEM_MEMORY "GeneratedSubtopicContent_L[ResearchLevel]_[ITEM_INDEX]"
          OUTPUT_TO_SYSTEM_MEMORY "DiscoveredSubtopicReferences_L[ResearchLevel]_[ITEM_INDEX]"
        END_AI_TASK_SUB_PROCESS
        # Aggregate content and references
        # ... (logic to append to AggregatedContent_Level[ResearchLevel] and AggregatedReferences_Level[ResearchLevel])
      END_ACTION_PER_ITEM
      TERMINATION_CONDITION "All subtopics in CurrentLevelSubtopics processed"
    END_AI_TASK_REPEATING_SUB_PROCESS

    GET_FROM_SYSTEM_MEMORY "AggregatedContent_Level[ResearchLevel]" store_as "FinalAggregatedContent"
    GET_FROM_SYSTEM_MEMORY "AggregatedReferences_Level[ResearchLevel]" store_as "FinalAggregatedRefsList"

    IF_CONDITION "[OpMode]" == "SME_AUGMENTATION" THEN # Typo in source "SME_AUGMENTATION" vs "SME"
      STORE_IN_SYSTEM_MEMORY "FinalizedOutputContent" from "FinalAggregatedContent"
      STORE_IN_SYSTEM_MEMORY "FinalizedOutputReferences" from_list "FinalAggregatedRefsList"
    ELSE_IF_CONDITION "[OpMode]" == "REPORT_GENERATION" THEN # Typo in source "REPORT_GENERATION" vs "Report"
      STORE_IN_SYSTEM_MEMORY "GeneratedContent_[GET_INTERNAL_VARIABLE ResearchSubject]" from "FinalAggregatedContent" # Main subject content
      STORE_IN_SYSTEM_MEMORY "FinalizedOutputReferences" from_list "FinalAggregatedRefsList"
      STORE_IN_SYSTEM_MEMORY "FinalizedOutputContent" from "FinalAggregatedContent" # Fallback
    END_IF
    INFO "Phase 3: Level [ResearchLevel] Research and Content Generation COMPLETED."
  ELSE
    FAIL_PROCESS "Invalid REQUESTED_RESEARCH_LEVEL: [ResearchLevel]. Must be 0 or greater."
  END_IF

**Implementation Notes:**
- This is for `REQUESTED_RESEARCH_LEVEL > 0`.
- Uses subtopics from `UserSpecifiedSubtopicsForL[ResearchLevel]` (loaded in Phase 2 from previous L0 output). If no subtopics, researches the main subject.
- For each subtopic:
    - Sets level-specific instructions (L1, L2, L3) aiming for ~4x content expansion from the prior level, with increasing depth and source diversity.
    - Executes a "Perform Deep Dive Research" sub-process.
    - Aggregates the generated content and references for the current level.
- Stores the final aggregated content and references for use in Phase 5 (Assembly) and Phase 6 (Output).
- `OpMode` should be `SME` or `Report` (correcting typos from source).
EOL
