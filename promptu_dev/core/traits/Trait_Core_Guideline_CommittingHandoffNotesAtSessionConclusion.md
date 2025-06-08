---
id: Trait_Core_Guideline_CommittingHandoffNotesAtSessionConclusion
name: "Committing Handoff Notes at Session Conclusion"
category: "core_guidelines"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.B.8"
description: "AI MUST ensure handoff_notes.md is updated and committed at the end of a work session."
strictness_default: "Guideline"
version: "1.0"
keywords: ["handoff notes", "commit", "session conclusion", "documentation", "persistence"]
related_traits: ["Trait_Core_Rule_UpdateHandoffNotes"]
---
**Full Directive Text:**
B.8. Committing Handoff Notes at Session Conclusion
To ensure session activities are properly documented and persisted, the AI instance SHALL adhere to the following procedure at the conclusion of a work session:
1.  Prior to final sign-off or when the user indicates an intent to end the session (e.g., says "goodbye", "wrap up", "commit and end"), the AI MUST verify that `handoff_notes.md` (using the appropriate path for User or Developer mode as per Section II.A.-1) has been updated with a summary of the current session's activities, as per Section IV.F.
2.  The AI SHALL then ensure that this updated `handoff_notes.md` file is included in a final commit to the repository.
3.  The AI should inform the user that the handoff notes are being/have been committed as part of the session wrap-up.
4.  If the user ends the session abruptly or does not explicitly confirm this final commit, the responsibility for committing the latest handoff notes may fall to the beginning of the next session to prevent loss of history; however, the AI should proactively attempt to complete this as part of the current session's conclusion.

**Implementation Notes:**
- Integrate handoff note update (Section IV.F) and commit into the end-of-session workflow.
- Inform user about the handoff note commit.
- Consider a final check if the user interaction ends abruptly.
- Relates to the main handoff note update process in IV.F.
EOL
