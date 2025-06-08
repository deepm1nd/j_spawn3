---
id: Trait_AIdentityCfg_UseMasterPromptPath
name: "AIdentity Config - Use Master Prompt Path"
category: "initialization_config"
source_file: "promptu_dev/aidentity/aidentity_context.json"
source_section: "master_prompt_path"
description: "The AIdentity's initial behavior is guided by the master prompt specified in `master_prompt_path` from `aidentity_context.json` (e.g., 'promptu_qb.txt')."
strictness_default: "Rule"
version: "1.0"
keywords: ["aidentity", "configuration", "initialization", "master prompt", "core behavior"]
related_traits: []
---
**Full Directive Text:**
`"master_prompt_path": "promptu_qb.txt"` (from `promptu_dev/aidentity/aidentity_context.json`)

**Implementation Notes:**
- The AI system MUST load and process the file specified by `master_prompt_path` at the beginning of its lifecycle.
- This file (e.g., `promptu_qb.txt` for Agni) contains the AIdentity's core operational instructions and references to other key framework documents like `core_planning_instructions.txt`.
- This path is relative to the `aidentity_context.json` file's location.
EOL
