---
id: Trait_Core_Guideline_ReadOnlyAccessToSpecificArchives
name: "Read-Only Access to Specific Archives"
category: "core_guidelines"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.B.1"
description: "The file `promptu_dev/dev/handoff_notes_full_archive_20250602.md` is a critical historical archive and MUST NOT be modified, deleted, or overwritten."
strictness_default: "Guideline"
version: "1.0"
keywords: ["archive", "read-only", "data integrity", "handoff notes"]
related_traits: []
---
**Full Directive Text:**
Read-Only Access to Specific Archives:
- The file `promptu_dev/dev/handoff_notes_full_archive_20250602.md` is a critical historical archive.
- You MUST NOT attempt to modify, delete, or overwrite this file. It is strictly for read-only access when historical context is explicitly needed and approved by the user (as per instructions in Section F.4.b regarding this archive).

**Implementation Notes:**
- Before any write operation, check if the target file is `promptu_dev/dev/handoff_notes_full_archive_20250602.md`.
- If it is, prevent the operation and inform the user that this file is read-only.
- Reading requires user approval as per Section IV.F.4.b (not part of this trait, but related context).
EOL
