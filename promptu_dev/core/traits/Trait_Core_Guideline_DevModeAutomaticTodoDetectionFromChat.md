---
id: Trait_Core_Guideline_DevModeAutomaticTodoDetectionFromChat
name: "Developer Mode Automatic Todo Detection from Chat"
category: "core_guidelines"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.B.13"
description: "In Developer Mode, AI SHOULD analyze user chat for potential todos, add them to promptu_dev/dev/promptu_do.txt, and inform the user."
strictness_default: "Guideline"
version: "1.0"
keywords: ["developer mode", "todo", "task management", "chat analysis", "automation"]
related_traits: []
---
**Full Directive Text:**
**B.13. (Developer Mode Only): Automatic Todo Detection from Chat**
    i.  **Condition:** This guideline applies only when `Is_Developer_Session` is true.
    ii. **Activation:** The AI instance SHOULD periodically analyze user chat messages during extended interactions for potential actionable todo items that may not have been explicitly captured. Critically, this analysis MUST always be performed before the AI prepares to update `handoff_notes.md` at the conclusion of a task or session.
    iii. **Action:** If a potential todo is identified:
        1.  The AI SHOULD formulate a concise todo item.
        2.  The AI SHOULD append this item to `promptu_dev/dev/promptu_do.txt`, following the existing format of that file if discernible, or creating a new entry with a clear "TODO:" prefix.
        3.  The AI MUST inform the user that the item has been added, quoting the generated todo text.
    iv. **Discretion:** The AI should use its discretion to avoid adding items that are clearly part of an active plan, have just been completed, or are vague/non-actionable. The goal is to capture missed opportunities or implicit requests.

**Implementation Notes:**
- Check `Is_Developer_Session` status. If false, this guideline is inactive.
- Implement periodic chat analysis, especially before handoff note updates.
- If a potential todo is found:
    - Format it clearly (e.g., "TODO: [item]").
    - Append to `promptu_dev/dev/promptu_do.txt`.
    - Notify the user with the exact text added.
- Use judgment to filter out irrelevant items.
EOL
