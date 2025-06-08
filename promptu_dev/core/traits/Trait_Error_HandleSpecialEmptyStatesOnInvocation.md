---
id: Trait_Error_HandleSpecialEmptyStatesOnInvocation
name: "Determine Invocation Context and Handle Special Empty States"
category: "error_handling_special_conditions"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "II.A.0"
description: "Handles scenarios at invocation: pre_promptu with no request, or post_promptu with no request/app/add-ons. Instructs AI to engage user for clarification and HALT."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "invocation context", "empty state", "user interaction", "error handling", "pre_promptu", "post_promptu"]
related_traits: ["Trait_Init_ExtractUserProjectRequest", "Trait_Init_ParseUserPromptAppSelection", "Trait_Init_DiscoverLoadAddons"]
---
**Full Directive Text:**
0.  **Determine Invocation Context and Handle Special Empty States:**
    *   Assume a variable `Invocation_Context` is available, set by the system to either 'PRE_PROMPTU' (if `pre_promptu.txt` is the entry point) or 'POST_PROMPTU' (if `post_promptu.txt` is the entry point). If unavailable, default to 'POST_PROMPTU' and issue a warning.
    *   Check `User_High_Level_Project_Goal` (from Section I.C), `Selected_PromptAppName` (from Section I.I), `Active_Primary_Addon_AppName` (from Section I.F), and `Inheritable_Addons_Content_Ordered_List` (from Section I.F).
    *   **Scenario 1 (pre_promptu with no request):**
        *   IF `Invocation_Context` is 'PRE_PROMPTU' AND (`User_High_Level_Project_Goal` is null OR `User_High_Level_Project_Goal` is empty_or_whitespace_marker):
            *   INFO: 'pre_promptu.txt' invoked without a user project request.
            *   INSTRUCT AI: 'Your primary task is to engage the user in a discussion to elicit their requirements or the objectives they wish to achieve with pre-processing utilities. Ask open-ended questions to understand their needs. Do not proceed with any utility selections from `[[USER_UTIL_SELECTION]]` in `pre_promptu.txt` until these initial requirements are clarified. HALT further framework processing and await user interaction.'
            *   RETURN (do not proceed to normal execution logic).
    *   **Scenario 2 (post_promptu with no request, no app, no add-ons):**
        *   ELSE IF `Invocation_Context` is 'POST_PROMPTU' AND (`User_High_Level_Project_Goal` is null OR `User_High_Level_Project_Goal` is empty_or_whitespace_marker) AND `Selected_PromptAppName` is null AND `Active_Primary_Addon_AppName` is null AND `Inheritable_Addons_Content_Ordered_List` is empty:
            *   INFO: 'post_promptu.txt' invoked without a user project request and no apps or add-ons selected.
            *   INSTRUCT AI: 'The framework was invoked without a specific project request or any selected app or add-on. Your task is to engage the user in a discussion to elicit their project requirements or what they intended to do. Ask open-ended questions to understand their project goals or if they need guidance on using the framework. HALT further framework processing and await user interaction.'
            *   RETURN (do not proceed to normal execution logic).
    *   **Scenario 3 (post_promptu with no request, but app selected):**
        *   ELSE IF `Invocation_Context` is 'POST_PROMPTU' AND (`User_High_Level_Project_Goal` is null OR `User_High_Level_Project_Goal` is empty_or_whitespace_marker) AND `Selected_PromptAppName` is not null AND `Available_PromptApps_Manifest_Map` contains `Selected_PromptAppName` AND `Selected_App_Tasks_Ordered` is not empty:
            *   INFO: 'post_promptu.txt' invoked without a user project request, but a promptApp '[Selected_PromptAppName]' is selected. Proceeding with promptApp execution.
            *   (Fall through to the existing promptApp execution logic, which will begin at step II.A.1. The selected promptApp component will be responsible for handling the missing `User_High_Level_Project_Goal` if it's critical for its operation.)
    *   ELSE:
        *   INFO: Proceeding with standard execution path.
        *   (Fall through to existing logic at step II.A.1)

**Implementation Notes:**
- This logic runs early in the execution sequence.
- Requires `Invocation_Context` (PRE_PROMPTU or POST_PROMPTU).
- Checks key variables: `User_High_Level_Project_Goal`, `Selected_PromptAppName`, `Active_Primary_Addon_AppName`, `Inheritable_Addons_Content_Ordered_List`.
- Scenario 1 & 2: If critical inputs are missing, AI is instructed to chat with user for clarification and HALT.
- Scenario 3: Allows proceeding with a PromptApp even if global project goal is missing.
- Otherwise, normal execution proceeds.
EOL
