---
id: Trait_CreateResearchReport_ParameterDefinitionAndResolution
name: "CreateResearchReport - Parameter Definition and Resolution"
category: "component_behaviors_create_research_report"
source_file: "promptu_dev/components/add_ons/create_research_report/create_research_report.txt"
source_section: "Phase 1: PARAMETER_DEFINITION_BLOCK and resolution loop"
description: "Defines the parameters for create_research_report and the process for resolving them from USER_CONFIG or internal defaults, including validation and handling of required parameters."
strictness_default: "Rule"
version: "1.0"
keywords: ["create_research_report", "configuration", "parameters", "initialization", "validation"]
related_traits: ["Trait_CreateResearchReport_ValidateParamsAndConstructPaths"]
---
**Full Directive Text:**
# Phase 1: Initialization and Configuration
PARAMETER_DEFINITION_BLOCK PARAMS_TO_RESOLVE
    # ParameterName | SourceType (USER_CONFIG or INTERNAL_DEFAULT) | DefaultValue (if SourceType is INTERNAL_DEFAULT) | IsRequired (true/false) | ValidationType
    RESEARCH_INPUT | USER_CONFIG | "[User prompt/topic or path/URL]" | true | NON_EMPTY_STRING_OR_PATH_URL
    OUTPUT_MODE | USER_CONFIG | "SME" | true | ENUM(SME,Report)
    TARGET_SME_DOMAIN_NAME | USER_CONFIG | "[SME Domain Name]" | false | NON_EMPTY_STRING # Required if OUTPUT_MODE is SME
    REPORT_IDENTIFIER | USER_CONFIG | "[Report Identifier]" | false | NON_EMPTY_STRING # Required if OUTPUT_MODE is Report
    REQUESTED_RESEARCH_LEVEL | USER_CONFIG | "0" | true | INTEGER_RANGE(0,3)
    REFERENCE_FOLLOWING_DEPTH | USER_CONFIG | "1" | true | INTEGER_RANGE(0,2)
    TARGETED_REFERENCE_SOURCES | USER_CONFIG | "[N/A]" | false | NONE
    PROPOSED_TABLE_OF_CONTENTS | USER_CONFIG | "[No Table of Contents Provided]" | false | NONE
    GENERAL_INPUT_DOCS | USER_CONFIG | "[No general input documents provided]" | false | PATH_LIST_EXISTS_OR_URL_LIST
    GUIDELINES_DOC_PATHS | USER_CONFIG | "[No Guidelines Provided]" | false | PATH_LIST_EXISTS_OR_URL_LIST
    ADDITIONAL_WEB_LINKS_FILE | USER_CONFIG | "[No Additional Web Links File Provided]" | false | PATH_EXISTS_OR_URL
    EXEMPLAR_DOCS | USER_CONFIG | "[No Exemplar Documents Provided]" | false | PATH_LIST_EXISTS_OR_URL_LIST
    PREVIOUS_ITERATION_OUTPUT_PATH | USER_CONFIG | "[Path to previous iteration state or N/A]" | false | PATH_EXISTS_OR_PLACEHOLDER_OR_NA
END_PARAMETER_DEFINITION_BLOCK

AI_TASK_REPEATING_SUB_PROCESS "Resolve Configuration Parameters"
  INITIAL_STATE_FROM_SYSTEM_MEMORY "UNRESOLVED_PARAMS_LIST" from PARAMS_TO_RESOLVE
  ACTION_PER_ITEM "Resolve Parameter [ITEM_NAME]"
    # (Logic for checking if already resolved, getting definition, resolving from USER_CONFIG vs INTERNAL_DEFAULT, validation, handling required/optional, and specific PREVIOUS_ITERATION_OUTPUT_PATH logic based on ITERATIVE_OUTPUT_DEPTH)
    # ... (details in source) ...
  END_ACTION_PER_ITEM
  TERMINATION_CONDITION "All parameters in UNRESOLVED_PARAMS_LIST processed"
END_AI_TASK_REPEATING_SUB_PROCESS
# (Followed by storing resolved params into Internal_... variables)

**Implementation Notes:**
- Defines all expected parameters for `create_research_report`.
- Specifies a resolution process:
    - Check if parameter already resolved (e.g., iterative mode).
    - Attempt to get from `[[USER_CONFIG_FOR_create_research_report]]`.
    - Validate user-provided value.
    - If not valid or not provided, use default if optional, or fail if required.
    - Special handling for `PREVIOUS_ITERATION_OUTPUT_PATH` based on `ITERATIVE_OUTPUT_DEPTH`.
- Resolved parameters are stored in internal variables (e.g., `Internal_RESEARCH_INPUT`).
EOL
