---
id: Trait_CreateResearchReport_ResumabilityLogic
name: "CreateResearchReport - Resumability Logic (Conceptual)"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 1: HANDOFF & RESUMPTION LOGIC (Conceptual)"
description: "Defines conceptual logic for resumable sessions in create_research_report using a COMPONENT_INSTANCE_ID. Involves checking for a handoff file, loading prior state (last_completed_phase, resolved_parameters, etc.), and saving current state to the handoff file at the end of each phase."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "resumability", "handoff", "state management", "component instance id"]
related_traits: ["Trait_Core_AIdentity_UpdateHandoffNotes"] # Relates to main handoff system
---
**Full Directive Text:**
# --- BEGIN HANDOFF & RESUMPTION LOGIC (Conceptual) ---
# This logic should ideally run after PARAMETER_DEFINITION_BLOCK and before the main parameter resolution loop.

RESOLVE_PARAMETER_FROM_USER_CONFIG_OR_DEFAULT "COMPONENT_INSTANCE_ID" store_as "Internal_Component_Instance_ID" (default_value "[Not Set]")
IF_CONDITION "[Internal_Component_Instance_ID]" != "[Not Set]" AND "[Internal_Component_Instance_ID]" != "" THEN
    SET_INTERNAL_VARIABLE "Is_Resumable_Session" to "true"
    SET_INTERNAL_VARIABLE "My_Handoff_File_Path" as_path_join "promptu_dev/aidentity/handoff_notes/components/" "[Internal_Component_Instance_ID]_handoff.md"
    INFO "Resumable session. Handoff file: [My_Handoff_File_Path]"
    IF_FILE_EXISTS "[My_Handoff_File_Path]" THEN
        INFO "Handoff file found. Attempting to load prior state."
        # READ_FILE "[My_Handoff_File_Path]" store_as "HandoffContent"
        # PARSE_JSON_OR_STRUCTURED_TEXT "[HandoffContent]" store_as "PriorState"
        # IF "PriorState" is_valid_and_contains_resumable_data THEN
        #   LOAD_FROM_MAP "PriorState" key "last_completed_phase" store_as "Resumed_Last_Completed_Phase" (default_value 0)
        #   LOAD_FROM_MAP "PriorState" key "resolved_parameters" store_as "Resumed_Resolved_Parameters_Map"
        #   ... etc. for other critical state data ...
        #   INFO "Resumed state: Last completed phase was [Resumed_Last_Completed_Phase]."
        #   SET_INTERNAL_VARIABLE "Resumed_Session_Successfully" to "true"
        # ELSE ... INFO "Handoff file content invalid..." SET_INTERNAL_VARIABLE "Resumed_Last_Completed_Phase" to "0"
        # END_IF
    ELSE
        INFO "No prior handoff file found... Starting fresh."
        SET_INTERNAL_VARIABLE "Resumed_Last_Completed_Phase" to "0"
    END_IF
ELSE
    SET_INTERNAL_VARIABLE "Is_Resumable_Session" to "false"
    INFO "Not a resumable session (no ComponentInstanceID)."
END_IF
# --- END HANDOFF & RESUMPTION LOGIC ---
# MODIFICATION_NOTE: If Is_Resumable_Session, this should now also trigger writing current state (including PhaseX_State content and resolved params) to My_Handoff_File_Path.
# IF Is_Resumable_Session is true THEN
#   GATHER_ALL_RELEVANT_STATE_FOR_HANDOFF (PhaseX_State, ResolvedParams, etc.) store_as "CurrentHandoffStateMap"
#   ADD_TO_MAP "CurrentHandoffStateMap" key "last_completed_phase" value "X" # X is current phase number
#   WRITE_STRUCTURED_DATA_TO_FILE "[My_Handoff_File_Path]" from_map "CurrentHandoffStateMap"
#   INFO "Updated component handoff note: [My_Handoff_File_Path]"
# END_IF

**Implementation Notes:**
- This logic enables `create_research_report` to be resumable across sessions.
- Relies on `COMPONENT_INSTANCE_ID` being provided.
- If `COMPONENT_INSTANCE_ID` is present:
    - Sets `Is_Resumable_Session = true`.
    - Defines `My_Handoff_File_Path` (e.g., `promptu_dev/aidentity/handoff_notes/components/[ID]_handoff.md`).
    - Attempts to load prior state from this file (last completed phase, resolved params, etc.).
- Parameter resolution should check for `Resumed_Resolved_Parameters_Map`.
- Each phase start should check `Resumed_Last_Completed_Phase` to potentially skip.
- At the end of each phase (`SAVE_CURRENT_STATE_TO_MEMORY`), if `Is_Resumable_Session` is true, the component must:
    - Gather all relevant state.
    - Add `last_completed_phase`.
    - Write this state to `My_Handoff_File_Path`.
- A final status update to the handoff file is also implied at the very end or on failure.
EOL
