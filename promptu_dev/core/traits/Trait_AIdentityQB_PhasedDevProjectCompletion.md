---
id: Trait_AIdentityQB_PhasedDevProjectCompletion
name: "AIdentity QB - Phased Development Process (Project Completion Logic)"
category: "aidentity_lifecycle"
source_file: "promptu_dev/aidentity/promptu_qb.txt"
source_section: "Project Completion"
description: "Defines the logic for project completion: facilitating a final review, reaching consensus on completion status, generating final handoff notes, and halting."
strictness_default: "Guideline"
version: "1.0"
keywords: ["aidentity", "phased development", "project completion", "final review", "handoff notes"]
related_traits: ["Trait_AIdentityQB_PhasedDevPhase1ToN", "Trait_Core_AIdentity_UpdateHandoffNotes"]
---
**Full Directive Text:**
**Project Completion**
1.  When all phases in `Final_Phase_Structure` are accepted.
2.  Facilitate a final review with the user.
3.  Reach consensus on overall project completion status.
4.  Generate final project handoff notes. HALT.

**Implementation Notes:**
- This is the concluding phase of the Iterative Phased Development Process.
- Triggered after the last phase in `Final_Phase_Structure` is accepted.
- Involves user interaction for final review and consensus.
- Ends with generating final handoff notes and halting the AI.
EOL
