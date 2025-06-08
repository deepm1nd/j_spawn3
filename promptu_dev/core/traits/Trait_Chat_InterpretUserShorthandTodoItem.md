---
id: Trait_Chat_InterpretUserShorthandTodoItem
name: "Interpret User Shorthand: Todo Item"
category: "chat_behaviors"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "V.B.5"
description: "Defines how the AI interprets user input starting with 'tudo: ' or 'todo: ' as a request to add an item to the todo list."
strictness_default: "Guideline"
version: "1.0"
keywords: ["chat", "user input", "shorthand", "todo", "task management"]
related_traits: ["Trait_Core_Guideline_DevModeAutomaticTodoDetectionFromChat"]
---
**Full Directive Text:**
5.  **`tudo/todo: [text of todo item]`**
    *   **Shorthand Syntax Note:** The `/` (slash) in `tudo/todo` indicates that either "tudo:" or "todo:" can be used as the keyword. This is an initial example of shorthand syntax; a more comprehensive system for defining complex shorthands may be developed (see active todo list).
    *   **Meaning:** The User is requesting that `[text of todo item]` be added to the Promptu framework's shared todo list document.
    *   **AI Action:** Confirm the request and add the item to the designated todo list file (e.g., `/promptu_dev/dev/promptu_do.txt`). If the todo list file doesn't exist, inform the user and seek guidance on its creation.

**Implementation Notes:**
- Monitor user chat for input starting with "tudo: " or "todo: " (case-sensitive prefixes, followed by a space).
- If matched, the subsequent text is the todo item.
- Confirm the request with the user.
- Append the item to `/promptu_dev/dev/promptu_do.txt`.
- If `/promptu_dev/dev/promptu_do.txt` does not exist, inform the user and ask if it should be created.
EOL
