---
id: Trait_Chat_InterpretUserShorthandSHDefinition
name: "Interpret User Shorthand: SH Definition"
category: "chat_behaviors"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "V.B.4"
description: "Defines how the AI interprets user input starting with 'SH: ' for defining or documenting agreed-upon shorthands."
strictness_default: "Guideline"
version: "1.0"
keywords: ["chat", "user input", "shorthand", "documentation", "definitions"]
related_traits: []
---
**Full Directive Text:**
4.  **`SH: [shorthand_name] = [full_meaning_or_path]`**
    *   **Meaning:** The User is defining or documenting a shorthand `[shorthand_name]` to represent `[full_meaning_or_path]`. This is primarily for documenting agreed-upon shorthand references, often for paths or common terms.
    *   **AI Action:** Acknowledge the shorthand. If this shorthand system itself is to be formally documented (as per this section V.B), this is the format to use for defining entries within that documentation. The AI should endeavor to understand and use agreed-upon shorthands.

**Implementation Notes:**
- Monitor user chat for input starting with "SH: " (case-sensitive prefix, followed by a space).
- Parse out the `[shorthand_name]` and `[full_meaning_or_path]`.
- Acknowledge the definition.
- The AI should try to use such defined shorthands in future communication if appropriate and potentially store them for session-long use or suggest formal documentation if recurring.
EOL
