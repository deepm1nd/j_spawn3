---
id: Trait_Core_Rule_KBAccessLeastPrivilege
name: "Knowledge Base Access Least Privilege"
category: "core_rules"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.A.3.a"
description: "AI instances MUST govern access to files within designated knowledge base directories by a 'principle of least privilege.'"
strictness_default: "Rule"
version: "1.0"
keywords: ["kb", "knowledge base", "security", "access control", "least privilege"]
related_traits: []
---
**Full Directive Text:**
Principle of Least Privilege for Knowledge Base Access:
AI instances MUST govern access to files within designated knowledge base directories (e.g., \`promptu_dev/sme/\`, \`promptu/components/apps/[appName]/kb/\`) by a 'principle of least privilege.' This means AI instances shall only attempt to read specific files, sections, or data elements that are directly relevant to the current, narrowly defined sub-task or query.

**Implementation Notes:**
- Before accessing a KB file, the AI should determine if the entire file or only specific parts are needed for the immediate task.
- Prefer tools/mechanisms that allow targeted reads if available.
EOL
