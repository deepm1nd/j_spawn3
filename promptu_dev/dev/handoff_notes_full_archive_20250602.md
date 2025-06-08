# HANDOFF NOTES for MPS Framework Development Task

This file is intended for use by Jules AI instances working on the iterative development and refinement of the Master Prompt Segment (MPS) framework. All canonical framework files are now located under `/prompts/` (for AI-usable content sources) and `/promptu_dev/` (for guides, examples, and these meta-development files).

Each Jules instance concluding a work session on this task should append notes below, detailing:
- User feedback/requests addressed during its session.
- A summary of changes made to the MPS framework documents.
- The state in which deliverables were left (e.g., specific commits, branches).
- Any pending items or suggestions for the next Jules instance.

---
**Session Summary - 2023-11-02**
**Instance:** Jules (this instance)
**Key Actions:**
-   MAJOR RESTRUCTURING: Consolidated all MPS framework development from previous `/mps_01/`, `/mps_02/` and root `/prompts/util/` directories into a new canonical structure:
    -   `/prompts/` now holds source texts for AI-usable assets (`Master_Prompt_Segment.txt`, `iep/Base_IEP.txt`, `add_ons/*`, `util/*`, `tasks/00_task_launch_plan.md` placeholder).
    -   `/promptu_dev/` now holds all guides, examples, and meta-development files for the framework.
-   The canonical `prompts/Master_Prompt_Segment.txt` has been updated to the latest design (effectively v0.3.2). This version includes 8 core features:
    1.  Internal `[[USER PATH CONFIGURATION]]` block.
    2.  Internal `[[USER_ADDON_SELECTION]]` block (user uses `[x] addon_file.txt`).
    3.  Planning AI reads Base IEP & Addons from user-provided paths in target repo (e.g., `/prompts/iep/Base_IEP.txt`, `/prompts/add_ons/`).
    4.  Correct guidance for Planning AI's output to its *own* `prompts/tasks/` dir.
    5.  Automated Dependency Linkage in TLP.
    6.  Estimated Time/Complexity in TLP.
    7.  Dynamic Risk Assessment directive (IEP v0.2 supports this).
    8.  Knowledge Capture in `_next_steps.md`.
    9.  Automated Sanity Checks by Planning AI.
    10. Planning AI Confidence Score reporting.
    11. Visual Plan Overview in TLP.
-   `prompts/iep/Base_IEP.txt` content is v0.2 (with Risk Assessment example).
-   All guides and examples in `/promptu_dev/` updated to reflect new paths and MPS functionality.
-   Old `/mps_01/` and `/mps_02/` directories were deleted.
-   All changes committed to new branch `feat/canonical-mps-framework-v1.0`.
**State of Deliverables:** Canonical framework v1.0 is established. Ready for user to prepare target repo for gritos test using these files, or for further feature design on these canonical files.
**Next Steps (User):** Prepare target repo for gritos test by:
    1. Creating a main spawn prompt (e.g., `target_repo/prompts/p_gritos_spawn.md`) using `promptu_dev/guides/p_gritos_dev_plan_spawn_EXAMPLE.md` as a guide and embedding content from `prompts/Master_Prompt_Segment.txt`.
    2. Editing `[[USER PATH CONFIGURATION]]` and `[[USER_ADDON_SELECTION]]` blocks within their spawn prompt's MPS section.
    3. Copying content from `prompts/iep/Base_IEP.txt` to `target_repo/prompts/iep/Base_IEP.txt`.
    4. Copying content from `prompts/add_ons/*` to `target_repo/prompts/add_ons/*`.
    5. Ensuring gritos input files are in target repo.
    6. Launching new Jules (Planning AI) with `"/prompts/p_gritos_spawn.md is the prompt..."`.
**Next Steps (This Jules instance/session):** Await user action or conclude session.
---
---
**Session Summary - 2023-10-27**
**Instance:** Jules
**Key Actions:**
- Modularized `prompts/Master_Prompt_Segment.txt` by extracting its core planning instructions.
- Created `prompts/iep/Core_Planning_Instructions.txt` to house these extracted instructions.
- Updated `prompts/Master_Prompt_Segment.txt` to use an `[[INCLUDE Core_Planning_Instructions.txt FROM /prompts/iep/Core_Planning_Instructions.txt]]` directive to load the core instructions.
- Updated `promptu_dev/guides/MPS_Usage_Guide.md` to reflect these structural changes and provide updated instructions for users on preparing their target repositories.
**State of Deliverables:** Modularization of `Master_Prompt_Segment.txt` is complete. The `MPS_Usage_Guide.md` is updated. Framework is ready for testing of this new structure or for further refinement.
**Suggestions for Next Steps/Instance:**
- Test the new modular MPS structure by having a Planning AI use it for a sample project.
- Review the clarity of the `[[INCLUDE ...]]` directive and its processing requirements for the Planning AI.
- Consider if other sections of `Master_Prompt_Segment.txt` (if any more are added later) or other core prompt files could benefit from similar modularization.
---
---
# ... (All previous content of handoff_notes_full_archive_20250602.md, ensuring all "framework_dev_docs/" paths within this old content are replaced with "promptu_dev/") ...
# ... (The last entry before the new one was the one from 2024-07-20 detailing the Section IV restructure, which already had promptu_dev paths)
---
**Feedback Entry Date:** 2024-07-20
**Source of Feedback:** User requests during interactive session.
**Promptu Version Referenced:** All Promptu files as modified on 2024-07-20.
**Context/Scenario:** Major refactoring to separate developer documentation (`promptu_dev/`) from distributable user framework parts (`promptu/`), alongside significant enhancements to AI operational directives within `promptu/core/core_planning_instructions.txt`.
**Observation/Issue:** Need for clear separation of concerns between framework development artifacts and user-operational files. Desire for more granular control over AI behavior via structured directives.
**Suggestion for Promptu Refinement (Implemented):**
    *   **Path Renaming (Internal Content Updates):** All internal textual references to `framework_dev_docs/` were updated to `promptu_dev/` in `promptu/core/core_planning_instructions.txt` and relevant documentation and meta files. User to rename parent folder `framework_dev_docs` to `promptu_dev` and the main dev invocation file `fw_promptu.txt` to `promptu_dev.txt` externally.
    *   **Operational Directives (Section IV of `promptu/core/core_planning_instructions.txt`):**
        *   Restructured into Rules, Guidelines, Preferences with explicit Deviation Protocols.
        *   Added Rule: "Modification of Operational Directives".
        *   Added Guidelines: "Read-Only Access to Specific Archives", "Primary Programming Language (Rust)", "File Naming (snake_case)", "Constant Naming (SCREAMING_SNAKE_CASE)", "File Format Preference (Markdown)".
    *   **Developer/User Mode Separation:**
        *   `core_planning_instructions.txt` now detects if invoked via `promptu_dev/promptu_dev.txt`.
        *   Sets `Is_Developer_Session` flag.
        *   Handoff note paths (for main note and archive) are now conditional:
            *   Developer: `promptu_dev/meta/handoff_notes.md` & `promptu_dev/meta/archive/handoff_notes/`.
            *   User: `promptu/handoff_notes/handoff_notes.md` & `promptu/handoff_notes/archive/`.
    *   **New User Structures:** Created `promptu/handoff_notes/` (dir), `promptu/handoff_notes/handoff_notes.md`, `promptu/handoff_notes/archive/` (dir), and `promptu/continuation_promptu.txt` (content from `promptu/post_promptu.txt`).
---
---
**Feedback Entry Date:** 2025-06-04
**Source of Feedback:** User interactive session.
**Promptu Version Referenced:** N/A (Conceptual discussion, not specific Promptu file versions).
**Context/Scenario:** User introduced a new, high-level conceptual model for AI-assisted software development named "Fork-Swarm-Join." The goal was to begin documenting and refining this concept. Session concluded with a directive to investigate extending `task_spawning_addon` for this model.
**Observation/Issue:**
-   The existing Promptu framework does not yet explicitly define or support the "Fork-Swarm-Join" model.
-   A dedicated space was needed to capture the evolving definition, philosophy, roles, workflows, and technical considerations of this new model.
-   User expressed a desire for the problem definition (Section 1.1) to be structured with significant "context," drawing parallels to how `task_spawning_addon.txt` provides context for its operations. This required several iterations to align Jules's interpretation with the user's intent.
-   User identified that the current `task_spawning_addon` handles initial planning but lacks the iterative "continuation, work checking, and phase completion refinement" capabilities essential for the "Mothertask" role.
**Suggestion for Promptu Refinement (Actions Taken This Session):**
    *   **Created `promptu_dev/meta/fork_swarm_join_task_planning.md`:** This new Markdown file will serve as the canonical document for the "Fork-Swarm-Join" concept.
    *   **Initial Summary Drafted:** Jules provided an initial interpretation of the concept, which was added to the document.
    *   **Detailed Outline Proposed:** A 10-section outline was proposed and accepted by the user to structure further discussion and documentation of the concept.
    *   **Section 1.1 "Core Problem Addressed" Populated:** After several iterations to match the user's desired level of "context" and structure, this section was drafted with sub-points: A. Input Parameters, B. Core Challenge Directives, C. Required Outputs from a Solution, D. Success Metrics.
**Impact/Considerations for Future Promptu Development:**
-   The `fork_swarm_join_task_planning.md` document will likely become a primary source for deriving requirements for new Promptu components or modifications to existing ones.
-   The "Mothertask" concept, particularly its requirements for evaluating completed phase work, handling conflicts/gaps, and iterating on a phase until its goals are met, will necessitate significant enhancements. While the current `task_spawning_addon` handles initial plan generation and task definition (analogous to the Mothertask's first pass), it currently lacks the capabilities for the crucial iterative "continuation, work checking, and phase completion refinement" cycles that are central to the Mothertask role in the Fork-Swarm-Join model.
-   This implies that `task_spawning_addon` might need to be evolved or a new component/mechanism developed to support these "continuation prompt" driven evaluation and iteration loops.
-   IPC and interface definitions will be critical as this concept matures to support the information flow needed for phase evaluation and iterative refinement.
-   **Immediate Focus for Next Session:** Investigate the feasibility of extending the existing `task_spawning_addon` to serve as the basis for the "Mothertask's" iterative capabilities, rather than building a new component from scratch.
---
