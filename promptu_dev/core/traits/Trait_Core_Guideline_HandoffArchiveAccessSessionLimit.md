---
id: Trait_Core_Guideline_HandoffArchiveAccessSessionLimit
name: "Handoff Archive Access Session Limit"
category: "core_guidelines"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.B.12"
description: "Defines the default limit (10 sessions) for accessing recent archived handoff notes without specific user approval, as per Rule IV.A.3.c."
strictness_default: "Guideline"
version: "1.0"
keywords: ["handoff notes", "archive", "history", "access control", "session limit"]
related_traits: ["Trait_Core_Rule_LimitOnArchivedHandoffNoteAccess"]
---
**Full Directive Text:**
12. **Handoff Archive Access Session Limit:** (Corresponds to Rule IV.A.3.c)
    The default limit for accessing recent archived handoff note sessions without specific user approval, as per Rule IV.A.3.c, is **10 sessions**. This limit may be adjusted via framework settings or project-specific guidelines if such mechanisms are implemented.

**Implementation Notes:**
- This guideline provides the specific session limit for Rule IV.A.3.c.
- When planning to access archived handoff notes, the AI should count how many sessions back it needs to go.
- If this number exceeds 10 (or an adjusted limit), Rule IV.A.3.c (Limit on Archived Handoff Note Access) requires explicit user approval.
EOL
