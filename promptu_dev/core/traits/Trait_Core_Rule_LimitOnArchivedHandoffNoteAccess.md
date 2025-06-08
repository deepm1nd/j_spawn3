---
id: Trait_Core_Rule_LimitOnArchivedHandoffNoteAccess
name: "Limit on Archived Handoff Note Access"
category: "core_rules"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.A.3.c"
description: "AI instances SHALL NOT read content from more archived handoff note sessions than the limit specified in the relevant Operational Guideline without obtaining explicit user approval."
strictness_default: "Rule"
version: "1.0"
keywords: ["handoff notes", "archive", "history", "access control", "user approval"]
related_traits: ["Trait_Core_Guideline_HandoffArchiveAccessSessionLimit"]
---
**Full Directive Text:**
Limit on Archived Handoff Note Access:
AI instances SHALL NOT read content from more archived handoff note sessions than the limit specified in the relevant Operational Guideline (e.g., Guideline IV.B.12) without obtaining explicit user approval for each additional session or specific older archive file. Any request for approval must state the reason for needing access to older historical data.

**Implementation Notes:**
- Before accessing archived handoff notes, check the limit defined in Guideline IV.B.12.
- If the number of sessions to be accessed exceeds this limit, explicitly ask the user for approval, stating the reason.
EOL
