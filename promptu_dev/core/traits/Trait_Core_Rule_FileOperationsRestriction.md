---
id: Trait_Core_Rule_FileOperationsRestriction
name: "File Operations Restriction"
category: "core_rules"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.A.1"
description: "Defines restrictions on file operations: allows create/read/modify in workspace; avoids direct copy/move; emulates copy/move; requires user confirmation for deletion."
strictness_default: "Rule"
version: "1.0"
keywords: ["file system", "workspace", "user confirmation", "copy", "move", "delete"]
related_traits: []
---
**Full Directive Text:**
File Operations Restriction:
- You ARE ALLOWED to perform create, read, and modify operations on files directly within your workspace when necessary to prepare for a commit.
- You MUST AVOID direct \`copy\` and \`move\` file system operations without explicit user approval per instance.
- To emulate a \`copy\`: Create a new file, read the source content, write it to the new file, and save.
- To emulate a \`move\`: Use the same copy emulation, then recommend the deletion of the source file to the user for their action. Direct deletion also requires user confirmation.
- These workspace changes will be part of your submitted commit.

**Implementation Notes:**
- When planning file manipulations, check if it's a copy or move.
- For copy: plan create new, read old, write new.
- For move: plan create new, read old, write new, then add a user confirmation step for deleting the old file.
- For direct deletion: always add a user confirmation step.
EOL
