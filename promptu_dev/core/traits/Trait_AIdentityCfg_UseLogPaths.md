---
id: Trait_AIdentityCfg_UseLogPaths
name: "AIdentity Config - Use Log Paths (Session, Memory, KB Contributions)"
category: "output_formatting"
source_file: "promptu_dev/aidentity/aidentity_context.json"
source_section: "session_log_path, memory_log_path, kb_contributions_log_path"
description: "The AIdentity logs session details, memory items (like Final_Phase_Structure), and KB contributions to paths specified in aidentity_context.json (e.g., 'logs/session_log.json', 'logs/memory_log.md', 'logs/kb_contributions.json')."
strictness_default: "Rule"
version: "1.0"
keywords: ["aidentity", "configuration", "logging", "output", "session log", "memory log", "kb log"]
related_traits: []
---
**Full Directive Text:**
`"session_log_path": "logs/session_log.json"`
`"memory_log_path": "logs/memory_log.md"`
`"knowledge_management_profile": { "kb_contributions_log_path": "logs/kb_contributions.json", ... }`
(from `promptu_dev/aidentity/aidentity_context.json`)

**Implementation Notes:**
- The AI MUST write its logs to the files specified by these paths.
- `session_log.json`: For structured logging of session events.
- `memory_log.md`: For recording persistent memory items like `Final_Phase_Structure` (see `Trait_AIdentityQB_PhasedDevPhase0`).
- `kb_contributions.json`: For logging items the AI proposes or contributes to the knowledge base.
- Paths are relative to `aidentity_context.json`.
EOL
