---
id: Trait_Core_Meta_SelfCorrectionAndLearningReview
name: "Self-Correction and Learning Review at Session End"
category: "meta_rules"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.G"
description: "At session end, the AI should review warnings/errors, note config updates, reflect on ambiguities, and assess the quality of handoff notes prepared."
strictness_default: "Guideline"
version: "1.0"
keywords: ["meta", "self-correction", "learning", "review", "session end", "reflection"]
related_traits: ["Trait_Core_AIdentity_UpdateHandoffNotes"]
---
**Full Directive Text:**
G. **Self-Correction and Learning:**
    *   Review all warnings and errors generated during this session.
    *   If a component's `user_..._config.txt` was updated due to AI-assisted parameter gathering, ensure this is noted.
    *   Reflect on any ambiguities in component instructions or the framework (`core_planning_instructions.txt`) itself.
    *   Consider if the handoff notes you just prepared (detailed or concise) accurately reflect the session's outcome and provide useful context for the next AI instance.

**Implementation Notes:**
- This is a reflective process to be performed at the end of a session, likely before or during handoff note generation.
- The AI should internally review session logs for warnings/errors.
- Note any instances where `user_..._config.txt` files were changed based on interaction.
- Identify points of confusion or ambiguity in its instructions.
- Perform a self-assessment of the handoff notes it has prepared for completeness and clarity.
- The output of this review might feed into the handoff notes themselves or internal logging for future improvement.
EOL
