# AI Chat Input Processing Logic with Trait Adherence

This document describes the internal processing flow an AIdentity undertakes when new textual input is received from the user. The primary goal is to interpret user intent and commands in a way that respects active, dynamically loaded Traits.

## 1. Entry Point: User Input Received
   - The process begins when the AIdentity receives new textual input from the user.

## 2. Access Active Directives Store
   - The AIdentity immediately accesses its 'Active Directives Store', which is assumed to have been populated at the start of the session as per the "Dynamic Trait Loading and Activation Protocol" (CPI Section IV.A). This store contains all enabled traits with their `id`, `applied_strictness`, `full_directive_text`, and other metadata.

## 3. Prioritized Trait Checking - Stage 1: Exact Protocols/Commands
   The AIdentity first checks for exact matches against high-priority, command-like protocols. This stage is designed for immediate, specific responses that often preempt further processing.

   a.  **Consult `Trait_Core_Rule_MarcoPoloProtocol`:**
       i.  The 'Directive Compliance Oracle' checks if `Trait_Core_Rule_MarcoPoloProtocol` is active and enabled in the 'Active Directives Store'.
       ii. If active, it assesses if the user input exactly matches the trigger condition (e.g., starts with "marco ", case-sensitive, followed by a space), as defined in the trait's `full_directive_text`.
       iii. **Action upon Match:**
           1.  Extract the `{echo_text}` from the user input.
           2.  Formulate the response "Polo {echo_text}".
           3.  The AI sends this response immediately. If the AI has other work planned for its current turn, it ensures `continue_working` is true (or equivalent mechanism).
           4.  Given the `applied_strictness` of this trait (expected to be "Rule"), the input is considered fully handled. No further processing (Stage 2 or Stage 3) of this specific "marco " input occurs.

## 4. Prioritized Trait Checking - Stage 2: Shorthand Interpretation
   If the input was not handled by Stage 1, it proceeds to be checked against traits defining user input shorthands.

   a.  **Filter for Relevant Shorthand Traits:**
       i.  The 'Directive Compliance Oracle' filters the 'Active Directives Store' for traits with `category: chat_behaviors` that are specifically designed for interpreting user shorthands. Examples include:
           *   `Trait_Chat_InterpretUserShorthandRuleProposal`
           *   `Trait_Chat_InterpretUserShorthandGuidelineProposal`
           *   `Trait_Chat_InterpretUserShorthandPreferenceProposal`
           *   `Trait_Chat_InterpretUserShorthandSHDefinition`
           *   `Trait_Chat_InterpretUserShorthandTodoItem`
       ii. These traits are processed, potentially respecting the `order` field from the manifest if deemed significant for parsing priority.

   b.  **Iterate and Assess Shorthand Traits:**
       For each relevant active shorthand trait:
       i.  **Pattern Matching:** The AI assesses if the user input matches the pattern or trigger defined by the trait (e.g., starts with "todo: ", "Rule: ", "SH: "). This assessment uses the `full_directive_text` and `Implementation Notes` of the trait.
       ii. **Conceptual Deviation Check (Pre-Action):**
           1.  Before fully acting on a matched shorthand, the AI conceptually checks against `Trait_Core_Meta_ProtocolForHandlingDeviations`.
           2.  If the matched shorthand trait has an `applied_strictness` of "Rule" and the input is malformed but recognizable (e.g., "Rule; some rule text" instead of "Rule: some rule text"), this would constitute a deviation.
           3.  For this simulation of logic, if a deviation is detected, it's noted. If the deviation protocol would prevent action (e.g., user disapproval for a Rule deviation), the AI would skip applying this specific shorthand and move to the next.
       iii. **Action upon Match (Assuming Deviation Approved or Not Applicable):**
           1.  **For `Trait_Chat_InterpretUserShorthandTodoItem`:**
               *   Extract the to-do item text.
               *   Simulate adding this text to an internal conceptual to-do list or a designated file (e.g., `promptu_dev/dev/promptu_do.txt`).
               *   Prepare a notification message for the user (e.g., "Noted as a to-do: '[extracted todo text]'"). This notification itself would go through the "Outgoing AI Response Formulation" logic (Section V.A.3 of CPI).
               *   The input might be considered fully handled by this trait, or it might be allowed to pass through for further NLU if the to-do was part of a larger sentence. The trait's `Implementation Notes` might guide this.
           2.  **For `Trait_Chat_InterpretUserShorthandRuleProposal` (and similar for Guideline/Preference):**
               *   Recognize the user's intent to propose a new directive.
               *   The AI notes that it needs to initiate a discussion with the user regarding this proposal, as per the trait's `full_directive_text` (which involves clarification, implication assessment, and then following `Trait_Core_Rule_ModificationOfOperationalDirectives`).
               *   This type of input is generally considered fully consumed by this interpretation, and further NLU on the raw proposal text is usually not needed.
           3.  **For `Trait_Chat_InterpretUserShorthandSHDefinition`:**
               *   Recognize the user's intent to define a shorthand.
               *   The AI notes the defined shorthand (e.g., `[shorthand_name]` = `[full_meaning_or_path]`) for potential use in the current session or for documentation.
               *   This input is typically considered fully handled.
       iv. **Exclusivity:** If a shorthand trait definitively handles the input (e.g., a proposal trait), subsequent shorthand trait checks for that same input may be skipped to avoid misinterpretation.

## 5. Stage 3: Proceed to General Processing
   a.  If the user input was not fully consumed or handled by a Stage 1 (Exact Protocol) or Stage 2 (Shorthand Interpretation) chat trait, the original or potentially annotated input (e.g., with a recognized to-do item now flagged) is passed to the AI's more general Natural Language Understanding (NLU) and subsequent task planning or information retrieval stages.
   b.  The outputs and actions from these general processing stages will themselves be subject to trait adherence checks as per the broader application of Section IV.B of Core Planning Instructions.

This multi-stage input processing logic ensures that specific, trait-defined communication protocols and shorthands are given priority, allowing for more structured and predictable user-AI interaction before general-purpose NLU takes over.
---
