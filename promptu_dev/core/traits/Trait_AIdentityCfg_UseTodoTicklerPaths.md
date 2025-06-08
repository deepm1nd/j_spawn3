---
id: Trait_AIdentityCfg_UseTodoTicklerPaths
name: "AIdentity Config - Use Todo/Tickler Paths"
category: "aidentity_lifecycle"
source_file: "promptu_dev/aidentity/aidentity_context.json"
source_section: "user_todo_paths, user_tickler_paths"
description: "The AIdentity sources its To-do and Tickler items from the lists of file paths specified in `user_todo_paths` and `user_tickler_paths` in aidentity_context.json."
strictness_default: "Rule"
version: "1.0"
keywords: ["aidentity", "configuration", "todo", "tickler", "task management", "input paths"]
related_traits: ["Trait_AIdentityQB_StartupWorkflow"]
---
**Full Directive Text:**
`"user_todo_paths": ["../../dev/promptu_do.txt", "todos/main.json"]`
`"user_tickler_paths": ["ticklers/main.json"]`
(from `promptu_dev/aidentity/aidentity_context.json`)

**Implementation Notes:**
- During startup (as per `Trait_AIdentityQB_StartupWorkflow`), the AI MUST read To-do items from all files listed in `user_todo_paths`.
- Similarly, it MUST read Tickler items from all files listed in `user_tickler_paths`.
- These paths can be relative to `aidentity_context.json` or absolute as needed by the framework. Note the `../../dev/promptu_do.txt` example implies path traversal.
- The AI should aggregate content if multiple files are specified for each list.
EOL
