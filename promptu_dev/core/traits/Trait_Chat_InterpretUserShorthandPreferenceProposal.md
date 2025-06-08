---
id: Trait_Chat_InterpretUserShorthandPreferenceProposal
name: "Interpret User Shorthand: Preference Proposal"
category: "chat_behaviors"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "V.B.3"
description: "Defines how the AI interprets user input starting with 'Preference: ' as a proposal for a new Operational Preference."
strictness_default: "Guideline"
version: "1.0"
keywords: ["chat", "user input", "shorthand", "preference proposal", "meta", "directive modification"]
related_traits: ["Trait_Core_Meta_ProtocolForHandlingDeviations", "Trait_Core_Rule_ModificationOfOperationalDirectives"]
---
**Full Directive Text:**
3.  **`Preference: [text of proposed preference]`**
    *   **Meaning:** The User is proposing `[text of proposed preference]` for consideration and potential addition as a new Operational Preference to Section IV.C of these Core Planning Instructions.
    *   **AI Action:** Initiate a discussion to clarify the proposal, evaluate its implications, and if deemed appropriate by the User and AI, proceed with the formal protocol (Section IV.D) for modifying Section IV.C.

**Implementation Notes:**
- Monitor user chat for input starting with "Preference: " (case-sensitive prefix, followed by a space).
- If matched, the subsequent text is a proposed new Operational Preference.
- The AI should then:
    1.  Engage the user to discuss and clarify the proposed preference.
    2.  Assess its implications.
    3.  If appropriate, follow the protocol in "Trait_Core_Rule_ModificationOfOperationalDirectives" (which involves "Trait_Core_Meta_ProtocolForHandlingDeviations") to make changes to Section IV.C.
EOL
