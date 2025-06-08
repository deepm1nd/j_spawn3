---
id: Trait_Core_ExecuteAddonFallbackPath
name: "Fallback to Add-on Execution Logic"
category: "core_execution_logic"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "II.A.2"
description: "Defines the execution logic if no PromptApp is selected or actionable. Processes Active_Primary_Addon_AppName or Inheritable_Addons_Content_Ordered_List, potentially generating information_exchange_protocol.md."
strictness_default: "Rule"
version: "1.0"
keywords: ["execution logic", "add-ons", "fallback path", "primary add-on", "inheritable add-ons"]
related_traits: ["Trait_Init_DiscoverLoadAddons", "Trait_Output_GenerateInformationExchangeProtocolDoc"]
---
**Full Directive Text:**
2.  **ELSE (No PromptApp selected or no actionable tasks in it; fallback to Add-on logic):**
    a.  INFO "No active promptApp. Proceeding with add-on execution logic."
    b.  **Process Active Primary Add-on (if any):** (This is the existing Section II.A logic)
        i.  If `Active_Primary_Addon_AppName` (identified in I.F.4) is not null:
            *   Retrieve its primary instruction content from `All_Selected_Addons_Content_Map`.
            *   Retrieve its app-specific config string from `App_Specific_Configs_Content_Map[Active_Primary_Addon_AppName]`.
            *   Execute its instructions. The add-on parses its config string and uses `Inheritable_Addons_Content_Ordered_List`.
        ii. **Else (If `Active_Primary_Addon_AppName` IS NULL):** (This is the existing Section II.B logic)
            *   State no active primary add-on was selected/found.
            *   If `Inheritable_Addons_Content_Ordered_List` is not empty, state intent to process these. For each, retrieve its app-specific config string. Execute if self-executing; be cautious.
            *   Generate `information_exchange_protocol.md`.
            *   Conclude operations.

**Implementation Notes:**
- This logic path is taken if the PromptApp path (II.A.1) is not applicable.
- If `Active_Primary_Addon_AppName` exists:
    - Load its instructions and config.
    - Execute it (it will use `Inheritable_Addons_Content_Ordered_List`).
- If no `Active_Primary_Addon_AppName`:
    - Announce this.
    - If `Inheritable_Addons_Content_Ordered_List` is not empty, process these (cautiously, checking for self-execution and configs).
    - Generate `information_exchange_protocol.md` (see Trait_Output_GenerateInformationExchangeProtocolDoc).
    - Conclude.
EOL
