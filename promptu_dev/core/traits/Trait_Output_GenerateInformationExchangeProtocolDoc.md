---
id: Trait_Output_GenerateInformationExchangeProtocolDoc
name: "Generate Information Exchange Protocol Document"
category: "output_formatting"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "III.C.1"
description: "Specifies conditions and procedure for generating the information_exchange_protocol.md document from base_iep.txt, typically if no primary add-on or PromptApp task runs to completion."
strictness_default: "Guideline"
version: "1.0"
keywords: ["output document", "iep", "information exchange protocol", "documentation"]
related_traits: ["Trait_Core_ExecuteAddonFallbackPath"]
---
**Full Directive Text:**
C. **Supporting Documentation (Core Utilities):**
1.  **`information_exchange_protocol.md`:**
    *   (Condition for generation as before, typically if no primary add-on or promptApp task successfully runs to completion of a major process).
    *   Load from `promptu/core/base_iep.txt`.
    *   Create in 'promptu folder' (user-configured output path, or a default like `outputs/current_session_prompts/`).
    *   (Example filenames for other docs: `dev_plan.md`, `mps_usage_guide.md`)

**Implementation Notes:**
- This document is generated conditionally. The main trigger is when no primary add-on or PromptApp task completes a major process (often part of the fallback path in II.A.2).
- Content is loaded from `promptu/core/base_iep.txt`.
- The output file is `information_exchange_protocol.md`.
- It should be placed in a user-configured output path or a default like `outputs/current_session_prompts/`.
EOL
