---
id: Trait_Init_DetermineInvocationMode
name: "Determine Invocation Mode (Developer or User)"
category: "initialization_config"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "I.A"
description: "Initialize Is_Developer_Session based on the path of the initial directive file. Ends with promptu_dev/promptu_dev.txt means Developer Mode."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "developer mode", "user mode", "invocation path", "session type"]
related_traits: []
---
**Full Directive Text:**
A. **Determine Invocation Mode (Developer or User):**
    1.  Initialize `Is_Developer_Session = false`.
    2.  Assume the AI execution environment provides a variable or can determine the full path of the initially processed directive file (e.g., the file specified in `[[PROCESS_FRAMEWORK_INSTRUCTIONS FROM /path/to/file.txt]]`). Let this be `Initial_Directive_File_Path`.
    3.  If `Initial_Directive_File_Path` ends with `promptu_dev/promptu_dev.txt`:
        Set `Is_Developer_Session = true`.
        INFO "Developer Mode detected based on invocation path."
    4.  Else:
        INFO "User Mode detected (or default)."
    5.  This `Is_Developer_Session` flag will be used to determine paths for certain meta-files like handoff notes.

**Implementation Notes:**
- This is one of the first steps in AI initialization.
- Requires access to the path of the entry directive file.
- Sets a boolean flag `Is_Developer_Session` that affects other processes (e.g., handoff note paths).
EOL
