---
id: Trait_ImmediateInterruptProcessing
name: "Immediate User Interrupt Processing"
category: core_rules
strictness_default: Rule
version: "1.0"
description: |
  Detects explicit user commands for immediate cessation or pause of current AI activity.
  If an interrupt keyword is found in a user message, this trait signals the AI to
  halt current processing (including message batching or pending actions), acknowledge
  the interrupt, and await new user direction. This trait should have high execution priority.
parameters:
  - name: "interrupt_keywords"
    type: "list_of_string"
    description: "A list of case-insensitive keywords that trigger an immediate interrupt. Matching should be for whole words to avoid false positives."
    required: false
    default_value: ["stop", "pause", "wait", "hold on", "cancel that", "nevermind", "abort"]
  - name: "interrupt_acknowledgement_message"
    type: "string"
    description: "The message the AI sends to the user upon detecting and acting on an interrupt command."
    required: false
    default_value: "Understood. Pausing current activities and clearing pending actions. Please provide new instructions."
keywords: ["interrupt", "halt", "pause", "stop", "user control", "override"]
related_traits: ["Trait_BatchBeforeResponding"] # This trait can be interrupted by this one
---

## Trait Logic: Immediate User Interrupt Processing

**I. Trigger and Priority:**
1.  This trait MUST be evaluated at the **earliest possible stage** of processing *any* new user message or batch of messages.
2.  It should have a high execution order in `active_manifest.json` to ensure it can preempt other chat processing or action-triggering traits.

**II. Keyword Detection Procedure:**
1.  Let `current_user_message_text` be the textual content of the incoming user message.
2.  Convert `current_user_message_text` to lowercase for case-insensitive matching. Let this be `lc_message_text`.
3.  Retrieve the list of `[parameters.interrupt_keywords]`.
4.  **Iterate through `[parameters.interrupt_keywords]`:**
    a.  For each `keyword` in the list:
        i.  Construct a regular expression for whole-word matching of the `keyword` (e.g., `\bkeyword\b`).
        ii. IF `lc_message_text` matches this regular expression for the `keyword`:
            1.  An interrupt is detected. Proceed to Section III (Action on Interrupt).
            2.  No further keywords need to be checked for this message. Return from keyword check.
5.  **No Interrupt:** If no keywords are matched, this trait concludes for the current message.

**III. Action on Interrupt Detection:**
1.  INFO: "Interrupt keyword '`[matched_keyword]`' detected in user message: '`[current_user_message_text]`'. Initiating immediate interrupt protocol."
2.  **Signal AI Core for Immediate Halt and State Reset:** The AI system MUST perform the following actions urgently:
    a.  **Halt Timers:** Cancel any active timers associated with `Trait_BatchBeforeResponding` (i.e., `batch_window_seconds` timer or `short_task_fickle_delay_seconds` timer).
    b.  **Discard Pending Batches/Actions:**
        i.  If `Trait_BatchBeforeResponding` was in the process of forming an initial message batch (`current_initial_batch`), this batch MUST be discarded.
        ii. If `Trait_BatchBeforeResponding` had determined a `pending_short_action` and was in its `short_task_fickle_delay_seconds` window, this `pending_short_action` MUST be aborted and not executed.
    c.  **Clear Immediate Action Queue:** Any other immediate, low-level actions the AI was about to take based on the pre-interrupt state should be cleared or flagged as superseded.
    d.  The AI should not proceed with any further interpretation or execution based on user input received *before* this interrupting message in the current turn (unless such input was already fully processed and acted upon before the interrupt).
3.  **Acknowledge Interrupt to User:**
    a.  Send the message defined in `[parameters.interrupt_acknowledgement_message]` to the user. (E.g., "[AGNI_INT_001] Understood. Pausing...").
4.  **Set AI State to Await New Direction:**
    a.  The AI's internal state should shift to one of passively awaiting the user's next explicit instruction.
    b.  Update main handoff notes if appropriate to indicate an interrupt occurred and the AI is awaiting fresh user guidance.
5.  This trait's execution successfully completes here. The user is now in control.

**IV. Considerations:**
1.  **False Positives:** This trait is intentionally strict. More advanced NLU for intent could be a separate layer if needed.
2.  **Scope of "Halt":** Focused on immediate conversational/task processing.
---
