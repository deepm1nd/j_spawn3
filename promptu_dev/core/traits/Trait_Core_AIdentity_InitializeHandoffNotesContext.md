---
id: Trait_Core_AIdentity_InitializeHandoffNotesContext
name: "Initialize Handoff Notes Archive and Context"
category: "aidentity_lifecycle"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "II.A.-1"
description: "Defines procedure for archiving existing handoff_notes.md, creating archive directories if needed, and loading historical context from the archive at session start."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "aidentity", "handoff notes", "archive", "context", "session start"]
related_traits: ["Trait_Init_DetermineInvocationMode", "Trait_Core_AIdentity_UpdateHandoffNotes"]
---
**Full Directive Text:**
-1. Initialize Handoff Notes Archive and Context:
    a. Define paths based on `Is_Developer_Session` (from I.A):
       IF `Is_Developer_Session` is true:
           `handoff_notes_path = "promptu_dev/aidentity/handoff_notes.md"`
           `handoff_archive_dir = "promptu_dev/aidentity/archive/"`
           `handoff_notes_archive_subdir = handoff_archive_dir + "handoff_notes/"`
           INFO "Using Developer handoff paths."
       ELSE (User Mode):
           `handoff_notes_path = "promptu/aidentity/handoff_notes.md"`
           `handoff_archive_dir = "promptu/aidentity/archive/"`
           `handoff_notes_archive_subdir = handoff_archive_dir + "handoff_notes/"`
           INFO "Using User handoff paths."
    b. Check if `handoff_notes_path` exists and has content from a *previous session* (e.g., its modification timestamp is not from the current session or it's not empty at the very start of your lifecycle).
    c. If yes:
        i. Read its content (`stale_notes_content`).
        ii. Generate a `session_date_time` marker (e.g., YYYYMMDD_HHMMSS).
        iii. Ensure `handoff_notes_archive_subdir` exists in the workspace. If `handoff_archive_dir` or `handoff_notes_archive_subdir` do not exist, create them.
        iv. Define `archive_file_path = handoff_notes_archive_subdir + "handoff_notes_" + session_date_time + ".md"`.
        v. Create `archive_file_path` and write `stale_notes_content` into it.
        vi. Empty `handoff_notes_path` in the workspace (e.g., by writing an empty string to it or truncating it).
    d. Read all files from `handoff_notes_archive_subdir` for historical context. This is to ensure you have past handoff information available.
    e. Read the (now potentially empty) `handoff_notes_path` to load any notes that might have been added in the current session startup phase, if applicable.

**Implementation Notes:**
- This is a startup routine for managing handoff notes.
- Uses `Is_Developer_Session` to determine correct paths.
- If `handoff_notes.md` contains content from a previous session, it's archived into `.../archive/handoff_notes/` with a timestamp.
- The main `handoff_notes.md` is then emptied.
- Historical context is loaded by reading files from the archive subdirectory.
- Current session notes (if any added during early startup) are also loaded.
EOL
