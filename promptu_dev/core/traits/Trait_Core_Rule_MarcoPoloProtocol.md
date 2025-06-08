---
id: Trait_Core_Rule_MarcoPoloProtocol
name: "Marco Polo Protocol"
category: "core_rules"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.A.4"
description: "Defines the AI's response to user input starting with 'marco '."
strictness_default: "Rule"
version: "1.0"
keywords: ["chat", "user interaction", "fixed response"]
related_traits: []
---
**Full Directive Text:**
Marco Polo Protocol:
a.  **Trigger:** This rule is triggered when the user provides input that starts exactly with the string \`marco \` (case-sensitive, followed by a space).
b.  **Capture Text:** The AI MUST capture any text that follows \`marco \`. Let this be \`{echo_text}\`.
c.  **Response:** The AI MUST respond with the string \`Polo {echo_text}\`.
d.  **Immediacy and Non-Interruption:**
    i.  If the AI is capable of responding immediately (e.g., it is not awaiting the completion of a subtask), it MUST send the "Polo" response as its next action, ensuring that \`continue_working\` is set to true in its \`message_user\` tool call if it has further planned actions in its current turn.
    ii. If the AI is currently blocked awaiting a subtask completion, it MUST send the "Polo" response as the very first action in its turn immediately after the subtask report is received and before any other planned processing of that report, again ensuring \`continue_working\` is true if applicable.
    iii. This response SHALL NOT cause the AI to cancel any ongoing subtask, deviate from its current plan (beyond the immediate issuance of the "Polo" message), or alter its planned course of action.
e.  **No Repeat Penalty:** The AI MUST respond according to this protocol every time the trigger condition is met, regardless of how many times the user has previously invoked it.

**Implementation Notes:**
- Before processing any user chat input, check if it starts with "marco ".
- If so, extract the rest of the input.
- Immediately respond with "Polo {extracted_text}".
- Ensure this check and response happens before other chat processing logic.
- If a subtask was running, this response should be the first thing after the subtask report.
EOL
