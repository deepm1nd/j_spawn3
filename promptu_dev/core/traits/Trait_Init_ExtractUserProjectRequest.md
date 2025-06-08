---
id: Trait_Init_ExtractUserProjectRequest
name: "Extract User High-Level Project Request"
category: "initialization_config"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "I.C"
description: "Extracts the User_High_Level_Project_Goal from between [[USER_PROJECT_REQUEST]] and [[END_USER_PROJECT_REQUEST]] delimiters in the current processing content."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "user request", "project goal", "parsing", "configuration"]
related_traits: []
---
**Full Directive Text:**
C. **Extract User's High-Level Project Request:**
    1.  Initialize `User_High_Level_Project_Goal`: `null` (or an empty string).
    2.  Define delimiters: `start_req_delim = "[[USER_PROJECT_REQUEST]]"`, `end_req_delim = "[[END_USER_PROJECT_REQUEST]]"`.
    1.  Search the *entire current processing content* (which the AI system is assumed to have prepared by combining the user's initial raw request with the user's configured `post_promptu.txt` file loaded via a directive) for text between these delimiters.
    4.  If found:
        a.  Extract the raw string content (exclusive of the delimiters, trim leading/trailing whitespace).
        b.  Store this extracted content as `User_High_Level_Project_Goal`.
        c.  INFO "User High-Level Project Goal successfully extracted."
    5.  If not found OR if the extracted content is empty or only whitespace:
        a.  WARNING "[[USER_PROJECT_REQUEST]] block not found or empty. The primary project goal may be missing. Subsequent planning processes might rely on user input provided through other configuration means (e.g., within component-specific parameters) or may require clarification."
        b.  `User_High_Level_Project_Goal` remains `null` or is set to a specific "not_found_or_empty" marker string.
    6.  The `User_High_Level_Project_Goal` (or its not-found status) should be accessible by primary directive components (e.g., the component for a selected promptApp task, or a selected primary add-on like `task_spawning_addon` or `create_research_report`) for their main planning or processing context. These components will be responsible for checking if the goal is sufficiently defined for their needs.

**Implementation Notes:**
- This process occurs during initialization.
- It involves searching the combined user request and `post_promptu.txt` content.
- The extracted `User_High_Level_Project_Goal` variable is critical for downstream components.
- Handle cases where the block is not found or is empty, and issue appropriate warnings.
EOL
