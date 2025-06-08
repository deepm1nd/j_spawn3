# Discussion: Fork-Swarm-Join Task Planning Concept

## Initial Summary (As understood by Jules on YYYY-MM-DD)

The core concept is a "fork-spawn-work-join" pattern for software development using multiple AI instances, orchestrated by a "mothertask."

1.  **Mothertask Initiation:**
    *   Starts with a detailed product specification (architecture, key components, code snippets, reference repos, etc.).
    *   Goal: Develop and implement a fully actionable plan to build the specified software.

2.  **Initial Planning by Mothertask:**
    *   Breaks the overall work into sequential **Phases**.
    *   Within each Phase, defines a **swarm of Tasks** designed for parallel execution by individual "worker" Jules instances.
    *   Ideally, the mothertask plans all Phases, Tasks, their interfaces, and necessary message structures upfront, if the initial information is complete enough.

3.  **Phase Execution Cycle:**
    *   **Fork & Spawn:** Worker Jules instances are assigned their parallel Tasks within the current Phase.
    *   **Work:** Each worker Jules instance completes its assigned Task, producing deliverables.
    *   **Join & Merge:** After all Tasks in a given Phase are reported as complete, the user (or another designated agent) is responsible for merging the work produced by the worker instances.
    *   **Mothertask Resumes (via Continuation Prompt):**
        *   Evaluates the integrated work done in the just-completed Phase.
        *   Plans and executes any remaining work required to fully meet that Phase's goals. This might include addressing conflicts that arose during the merge, processing items from Inter-Process Communication (IPC) channels, incorporating user feedback, etc.
        *   This "completion" work for a phase might itself be broken down into sub-phases with additional parallel tasks if needed.
        *   Continues this process until the current Phase's defined goals are met.

4.  **Iteration:** The Mothertask repeats the entire Phase Execution Cycle for all subsequent Phases until the overall project is complete.

5.  **Handling Incomplete Information / Planning Gaps:**
    *   If the mothertask determines it cannot plan everything upfront (e.g., due to undefined inputs or dependencies for a future phase):
        *   The user reviews the generated (partial) plan.
        *   **Option 1:** The user can accept the plan for the work that *can* be planned, deciding to defer detailed planning for the uncertain parts until a later stage (presumably when more information becomes available or the preceding phase's outputs clarify the situation). The mothertask would then be re-invoked for this subsequent planning.
        *   **Option 2:** The user might identify alternatives, provide the missing information, or make decisions that allow the mothertask to continue with more comprehensive planning.

This document will serve as a space to discuss, refine, and elaborate on this concept.

---
## Proposed Discussion Outline

To systematically explore and define the "Fork-Swarm-Join" concept, we can use the following structure:

### 1. Overall Philosophy and Goals
    *   **1.1. Core Problem Addressed**

*   **A. Input Parameters of the Problem:**
    *   Modern hardware and software engineering systems exhibit rapidly escalating complexity.
    *   There are increasingly stringent and often conflicting goals for performance, safety, security, and functionality.
    *   The capacity of human teams, even large and expert ones, is often exceeded by these demands, creating bottlenecks for innovation and timely delivery.

*   **B. Core Challenge Directives (Needs for a New Development Paradigm):**
    *   **Need for Advanced Decomposition:** Systematically break down massive and intricate requirements into clearly defined, manageable, and verifiable units of work.
    *   **Need for Effective Coordination:** Efficiently orchestrate the execution of these work units, maximizing parallelism while ensuring coherent integration of outputs.
    *   **Need for Deep AI Integration:** Embed AI capabilities throughout the development lifecycle, enabling AI to act as an active participant in design, coding, and complex problem-solving, rather than just a peripheral tool.
    *   **Need for a Scalable Framework:** Develop a structured approach that leverages AI to guide, direct, inform, and augment the development process, allowing teams to tackle problems previously considered intractable.

*   **C. Required Outputs from a Solution (The 'Fork-Swarm-Join' Aims):**
    *   A robust methodology where a central AI (the 'Mothertask') analyzes complex system requirements and decomposes the project into sequential **Phases**.
    *   Within each Phase, the Mothertask further defines a **swarm of Tasks**, including interface specifications and IPC mechanisms, suitable for independent and parallel execution by individual AI instances (Worker Jules).
    *   A managed lifecycle for these tasks (spawning, execution, monitoring, integration) that facilitates AI's active contribution to development.

*   **D. Success Metrics for Overcoming the Problem:**
    *   Demonstrable acceleration of development cycles for complex systems.
    *   Enhanced ability to design, implement, and verify systems against strict performance, safety, and security goals.
    *   Improved scalability of development efforts, allowing smaller teams augmented by AI to achieve results comparable to much larger traditional teams.
    *   Clear evidence of AI (Jules instances) successfully participating in and contributing to the code development process.
    *   1.2. Desired Outcomes and Benefits (e.g., speed, quality, scalability)
    *   1.3. Key Principles (e.g., parallelism, autonomy, clear interfaces)
    *   1.4. Overarching Vision: Context-Aware AI Application
        A key objective of the Promptu framework and concepts like Fork-Swarm-Join is to enable the development of AI-based applications that excel at creating, recalling, and utilizing CONTEXT. This contextual understanding is paramount for effective AI participation in complex development tasks.

### 2. Key Roles and Responsibilities
    *   2.1. **Mothertask AI**
        *   2.1.1. Initial Planning (Phases, Tasks, Interfaces)
        *   2.1.2. Phase Evaluation and Gap Analysis
        *   2.1.3. Conflict Resolution Planning
        *   2.1.4. Continuation Prompt Management
        *   2.1.5. Reporting and Status Updates
    *   2.2. **Worker AI (Jules Instance)**
        *   2.2.1. Task Execution
        *   2.2.2. Deliverable Generation
        *   2.2.3. IEP Adherence (Commits)
        *   2.2.4. IPC Usage (for non-commit data)
        *   2.2.5. Reporting Task Completion/Blockers
    *   2.3. **User/Reviewer/Integrator**
        *   2.3.1. Providing Initial Product Specification
        *   2.3.2. Reviewing and Approving Plans
        *   2.3.3. Merging Worker AI Deliverables
        *   2.3.4. Providing Feedback for Mothertask
        *   2.3.5. Resolving Ambiguities / Providing Missing Info

### 3. Detailed Workflow and Processes
    *   3.1. **Initial Setup & Mothertask Invocation**
        *   3.1.1. Inputs: Product Specification, Target Repository State
        *   3.1.2. Mothertask's First Pass: Full Project Plan (Ideal Scenario)
    *   3.2. **Phase Execution Cycle**
        *   3.2.1. Mothertask: Defining Phase Goals and Task Swarm
        *   3.2.2. Spawning Worker AIs (Mechanism TBD - e.g., manual, automated)
        *   3.2.3. Worker AI: Task Execution & Local Commits
        *   3.2.4. Worker AI: Signaling Task Completion
        *   3.2.5. User/Integrator: Merging Worker AI Branches/PRs
    *   3.3. **Mothertask Continuation & Phase Closure**
        *   3.3.1. Triggering Mothertask (Continuation Prompt)
        *   3.3.2. Mothertask: Analyzing Merged Work vs. Phase Goals
        *   3.3.3. Mothertask: Identifying Gaps, Conflicts, New Work
        *   3.3.4. Mothertask: Planning "Phase Completion" Sub-Tasks (if needed)
        *   3.3.5. Iterating Sub-Tasks until Phase Goals are Met
    *   3.4. **Project Completion and Finalization**

### 4. Information Requirements and Artifacts
    *   4.1. **Input Artifacts**
        *   4.1.1. Product Specification Document (Content, Structure)
        *   4.1.2. Code Repository (Initial State)
        *   4.1.3. User Feedback / Guidance
    *   4.2. **Mothertask-Generated Artifacts**
        *   4.2.1. Overall Project Plan (Phases, Task Definitions, Interface Specs)
        *   4.2.2. Phase-Specific Plans
        *   4.2.3. Task Prompts for Worker AIs (derived from `task_spawning_addon`?)
        *   4.2.4. Continuation Prompts (Structure, Content)
    *   4.3. **Worker AI-Generated Artifacts**
        *   4.3.1. Code / Deliverables
        *   4.3.2. Commit Messages (IEP)
        *   4.3.3. `_dev_plan.md`, `_next_steps.md`
        *   4.3.4. IPC Messages/Logs

### 5. Communication Protocols and Interfaces
    *   5.1. **Mothertask <-> Worker AI**
        *   5.1.1. Task Assignment (via Task Prompts)
        *   5.1.2. Reporting Progress/Completion (Mechanism TBD: e.g., specific file signals, API calls)
    *   5.2. **Worker AI <-> Worker AI (Indirect)**
        *   5.2.1. Via IPC Directory (`promptu/ipc/` or task-specific subdirs)
        *   5.2.2. Clearly Defined Data Structures/Formats for IPC
    *   5.3. **AI <-> User**
        *   5.3.1. Plan Reviews and Approvals
        *   5.3.2. Feedback Loops
        *   5.3.3. Requests for Clarification

### 6. Conflict Management and Resolution
    *   6.1. Proactive Conflict Avoidance (Mothertask's planning responsibility)
    *   6.2. Identifying Conflicts Post-Merge
    *   6.3. Mothertask's Role in Planning Conflict Resolution Tasks
    *   6.4. User's Role in Guiding Complex Conflict Resolution

### 7. Error Handling, Retries, and Resilience
    *   7.1. Worker AI Task Failure (Detection, Reporting)
    *   7.2. Mothertask Strategies for Re-assigning/Re-planning Failed Tasks
    *   7.3. Ensuring Idempotency where possible

### 8. Scalability and Limitations
    *   8.1. Number of Parallel Tasks
    *   8.2. Complexity of Product Specification
    *   8.3. Overhead of Merge/Integration
    *   8.4. Current Limitations of AI Models

### 9. Open Questions and Areas for Future Development
    *   (To be populated during discussion)

### 10. Relationship to Existing Promptu Framework
    *   10.1. How `task_spawning_addon` fits in or needs modification.
    *   10.2. Role of `core_planning_instructions.txt`.
    *   10.3. Use of `base_iep.txt`.
    *   10.4. New components/add-ons needed.

---
Please review this outline. We can then discuss and refine it.

---
### 11. Proposals for Extending `task_spawning_addon.txt` for Mothertask Iterative Capabilities

This section outlines proposals for modifying and extending the existing `promptu/add_ons/task_spawning_addon/task_spawning_addon.txt` and its configuration file `USER_task_spawning_addon_CONFIG.txt` to support the iterative evaluation and re-planning capabilities required by the "Mothertask" role in the Fork-Swarm-Join model.

**11.1. Introduce an Operational Mode Parameter (`TSA_MODE`)**

*   **Proposal:** Add a new mandatory parameter `TSA_MODE` to `promptu/add_ons/task_spawning_addon/USER_task_spawning_addon_CONFIG.txt`.
    *   Example entry in `USER_task_spawning_addon_CONFIG.txt`:
        ```
        # Requirement: Required | DefaultType: Accepted
        # Description: Specifies the operational mode. 'initial_planning' for generating the first full project plan. 'continuation_mothertask' for iterative phase evaluation and completion.
        TSA_MODE: [USER_VALUE_START] initial_planning [USER_VALUE_END]
        ```
    *   Accepted values: `initial_planning`, `continuation_mothertask`.
*   **Rationale:** This parameter will allow the `task_spawning_addon.txt` to fundamentally branch its execution logic. The behavior of most subsequent sections within the add-on will be conditional on this mode. When acting as the Mothertask for iterative refinement, its inputs, processing steps, and outputs will differ significantly from its initial planning role.

**11.2. Enhance Parameter System for `continuation_mothertask` Mode**

*   **Proposal:** Introduce new parameters in `USER_task_spawning_addon_CONFIG.txt`. These parameters would primarily be relevant and potentially required only when `TSA_MODE` is set to `continuation_mothertask`.
    *   `CONTINUATION_PREVIOUS_PHASE_PLAN_PATH`:
        *   `# Requirement: Required (if TSA_MODE='continuation_mothertask') | DefaultType: Placeholder`
        *   `# Description: Path to the original plan document or specific section defining goals for the phase now under review.`
        *   `CONTINUATION_PREVIOUS_PHASE_PLAN_PATH: [USER_VALUE_START] outputs/MyProject/planning_files/main_development_plan.md#Phase1_Goals [USER_VALUE_END]`
    *   `CONTINUATION_MERGED_CODE_SNAPSHOT_PATH`:
        *   `# Requirement: Required (if TSA_MODE='continuation_mothertask') | DefaultType: Placeholder`
        *   `# Description: Path to the root of the codebase snapshot representing the merged state of worker tasks for the phase under review.`
        *   `CONTINUATION_MERGED_CODE_SNAPSHOT_PATH: [USER_VALUE_START] /path/to/repo/after_phase1_merge [USER_VALUE_END]`
    *   `CONTINUATION_WORKER_ARTIFACTS_MANIFEST_PATH`:
        *   `# Requirement: Required (if TSA_MODE='continuation_mothertask') | DefaultType: Placeholder`
        *   `# Description: Path to a manifest file (e.g., JSON/YAML) listing key outputs from worker AIs of the previous swarm (e.g., paths to their _dev_plan.md, _next_steps.md, key IPC outputs, final commit hashes).`
        *   `CONTINUATION_WORKER_ARTIFACTS_MANIFEST_PATH: [USER_VALUE_START] outputs/MyProject/planning_files/phase1_worker_manifest.json [USER_VALUE_END]`
    *   `CONTINUATION_USER_FEEDBACK_FILE_PATH`:
        *   `# Requirement: Optional | DefaultType: Placeholder`
        *   `# Description: Optional path to a structured file containing user feedback on the completed phase.`
        *   `CONTINUATION_USER_FEEDBACK_FILE_PATH: [USER_VALUE_START] [USER_VALUE_END]`
    *   `CONTINUATION_PHASE_ID_UNDER_REVIEW`:
        *   `# Requirement: Required (if TSA_MODE='continuation_mothertask') | DefaultType: Placeholder`
        *   `# Description: A unique identifier for the specific phase currently being evaluated (e.g., "Phase1_SecurityFeatures").`
        *   `CONTINUATION_PHASE_ID_UNDER_REVIEW: [USER_VALUE_START] [USER_VALUE_END]`
*   **Rationale:** These new parameters provide the essential contextual inputs for the Mothertask to perform its evaluation and re-planning duties for a specific phase that has undergone initial work.
*   **Logic Change in Section I.B (Resolve Parameters) of `task_spawning_addon.txt`:** The parameter resolution logic would need to be updated to make these `CONTINUATION_` parameters conditionally required based on the `TSA_MODE`.

**11.3. Modify Core Planning Logic (Section II) & Introduce New Sections for `continuation_mothertask` Mode**

*   **Proposal (High-Level Structure in `task_spawning_addon.txt`):**
    ```
    IF TSA_MODE == 'initial_planning':
        [[Existing Section II directives for initial planning, largely as they are]]
    ELSE IF TSA_MODE == 'continuation_mothertask':
        [[New directives for Mothertask iterative processing: II-M, II-N, II-O, II-P]]
    END IF
    ```

*   **Proposed New Sections/Directives for `continuation_mothertask` Mode (to be inserted into `task_spawning_addon.txt`):**

    *   **II-M. INGEST AND ANALYZE PHASE ARTIFACTS (Continuation Mode):**
        *   Objective: To build a comprehensive understanding of the outcomes of the preceding swarm of tasks for the `CONTINUATION_PHASE_ID_UNDER_REVIEW`.
        *   Steps:
            1.  Load and parse the original goals for `CONTINUATION_PHASE_ID_UNDER_REVIEW` from the file/path specified in `CONTINUATION_PREVIOUS_PHASE_PLAN_PATH`.
            2.  Systematically analyze the codebase state provided at `CONTINUATION_MERGED_CODE_SNAPSHOT_PATH`. This may involve:
                *   Listing key modified files/modules.
                *   Potentially performing static analysis or calling external scripts for specific checks if defined by the phase goals.
                *   If a "baseline" or "previous commit" state is available or deducible, perform a diff analysis to understand changes.
            3.  Parse the `CONTINUATION_WORKER_ARTIFACTS_MANIFEST_PATH` to gather structured information from previous worker AIs (e.g., completion status, links to critical `_next_steps.md` sections, reported IPC data).
            4.  Parse the `CONTINUATION_USER_FEEDBACK_FILE_PATH` (if provided) to extract actionable feedback items.
            5.  **Output of this section:** An internal, structured representation of the "current observed state" of the phase, compared against its "original desired state."

    *   **II-N. GAP AND CONFLICT IDENTIFICATION (Continuation Mode):**
        *   Objective: To pinpoint discrepancies between the observed state and desired state, and to identify any new issues.
        *   Steps:
            1.  Compare the "current observed state" (from II-M) against the "original desired state."
            2.  Identify and list:
                *   Unmet or partially met requirements/goals.
                *   Regressions or new issues introduced in the codebase.
                *   Merge conflicts that were auto-resolved sub-optimally or require further manual/AI review and correction.
                *   Actionable items from worker AI `_next_steps.md` or IPC logs that require further work or integration.
                *   Specific points from user feedback that need to be addressed.
            3.  **Output of this section:** A structured list of "Deltas, Gaps, and Issues" with priorities if possible.

    *   **II-O. PLAN PHASE COMPLETION TASKS (Corrective/Supplementary Swarm - Continuation Mode):**
        *   Objective: To generate a plan of action to address all items identified in II-N.
        *   Steps:
            1.  For each significant item in the "Deltas, Gaps, and Issues" list, define one or more specific, actionable tasks to address it.
            2.  These tasks should be granular, well-defined, and designed for parallel execution where possible (a "corrective swarm").
            3.  Apply merge conflict mitigation strategies during the definition of these new tasks.
            4.  Estimate effort and assess confidence for this "Phase Completion Plan."
            5.  **Output of this section:** A "Phase Completion Plan" document (this could be a new, temporary plan or an update/addendum to an existing plan document for the phase) and a new set of task prompt files for the corrective swarm.

    *   **II-P. ITERATION CONTROL AND PHASE COMPLETION DETERMINATION (Continuation Mode):**
        *   Objective: To decide if the phase is complete or if another iteration is needed.
        *   Logic:
            1.  **IF** the "Deltas, Gaps, and Issues" list (from II-N) is empty or contains only very minor, acceptable deviations **AND** all critical user feedback has been addressed:
                *   Declare `CONTINUATION_PHASE_ID_UNDER_REVIEW` as **complete**.
                *   Generate a final status report for the completed phase.
                *   **Guidance for Next Phase:** If subsequent phases are defined in the overall project plan, provide instructions to the user on how to initiate the Mothertask for the *next* phase. This might involve:
                    *   User updating a master project status file.
                    *   User re-invoking `task_spawning_addon` in `initial_planning` mode but with parameters contextualized for the *next* phase (e.g., `PROJECT_NAME=MyProject_Phase2`).
                    *   Or, potentially a new `TSA_MODE` like `initiate_next_phase`.
                *   HALT current execution.
            2.  **ELSE (if corrective tasks were planned in II-O):**
                *   Proceed to generate the outputs (as per modified Section III) for the corrective swarm (i.e., the "Phase Completion Plan" and new task prompts).
                *   Provide clear instructions to the user on how to launch this "corrective swarm" of tasks.
                *   State that after the corrective swarm's work is completed and merged by the user, the `task_spawning_addon` (as Mothertask) should be re-invoked in `continuation_mothertask` mode, referencing the *same* `CONTINUATION_PHASE_ID_UNDER_REVIEW`. This forms the iterative loop.
                *   HALT current execution.

**11.4. Modify Output Generation (Section III of `task_spawning_addon.txt`) for `continuation_mothertask` Mode**

*   **Proposal:** Section III needs conditional logic based on `TSA_MODE`.
    *   When `TSA_MODE == 'continuation_mothertask'`:
        *   **Main Development Plan Document (III.A):** Instead of generating/overwriting the main project plan, it might:
            *   Generate a "Phase Status & Completion Plan" document specific to the `CONTINUATION_PHASE_ID_UNDER_REVIEW` and the current iteration.
            *   Or, update/append to a section within the existing `Main Development Plan` pertaining to this phase.
        *   **Task Prompt Files (III.B):** Generate task prompts for the "corrective/supplementary swarm" as defined in II-O.
            *   Naming convention for these prompts might include an iteration number, e.g., `p<PhaseID>_iter<N>_t<Task#>_description.txt`.
            *   Output paths for these prompts and their artifacts might include an iteration-specific subdirectory, e.g., `.../[TASK_PROMPTS_SUBDIR]/[PHASE_ID]_iteration_N/`.
        *   **Task Launch Plan (III.C.1):** Generate a TLP specifically for the corrective swarm. Its title and overview should reflect that it's for a corrective iteration of a specific phase.
*   **Rationale:** Outputs must be contextual to the iterative refinement process, providing clear instructions for the next set of actions (launching a corrective swarm) and status for the ongoing phase.

**11.5. Adapt Self-Correction Logic (Section IV of `task_spawning_addon.txt`)**

*   **Proposal:** The mandatory self-correction checks (Section IV.A) performed by the Planning AI (Mothertask) on its generated task prompts would need to be:
    *   Extended to cover any new types of prompts or plans generated in `continuation_mothertask` mode.
    *   Potentially have conditional checks based on `TSA_MODE`.
*   **Rationale:** Ensuring the integrity and correctness of prompts generated for corrective swarms is just as crucial as for initial swarms.

**11.6. Interface with Core Instructions (`core_planning_instructions.txt`) and Invocation**

*   **Consideration:** How is the `task_spawning_addon` invoked in `continuation_mothertask` mode, and how are the new `CONTINUATION_` parameters passed?
*   **Proposal (Leverage Existing Mechanisms):**
    *   The user would prepare a "Continuation Prompt" file (e.g., `continue_phase1_prompt.txt`).
    *   This prompt file would contain a `[[USER_CONFIG_FOR_task_spawning_addon]]` block.
    *   Inside this block, the user would set `TSA_MODE: continuation_mothertask` and provide all other required `CONTINUATION_` parameters.
    *   The `core_planning_instructions.txt` (Section I.D.4.c.iii) already has logic to parse such blocks and make them available to the add-on. This seems like the most straightforward way to provide the necessary context without needing changes to `core_planning_instructions.txt` itself for invocation, as it would treat it like any other add-on invocation just with different parameters.
*   **Rationale:** Reusing the existing parameter block mechanism is efficient and requires minimal changes to the core framework invocation logic. The primary changes are internal to `task_spawning_addon.txt` itself.

These proposals aim to provide a comprehensive starting point for evolving `task_spawning_addon.txt` into a component capable of fulfilling the iterative aspects of the Mothertask role. Each point would require further detailed design during an implementation phase.
---
