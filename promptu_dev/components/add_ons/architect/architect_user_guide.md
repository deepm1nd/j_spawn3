```markdown
# `dev_planner` Add-on: User Guide

## 1. `dev_planner` Add-on: Introduction & Mission

**Purpose:**
The `dev_planner` add-on is a sophisticated component within the Promptu Framework designed to generate comprehensive, phased, and conflict-minimized development plans. It takes high-level project goals and detailed specifications as input and produces actionable task prompts for subsequent AI developer instances.

**Vision:**
`dev_planner` aims to embody evolving expertise in systems engineering and project planning. It's designed to not only break down complex tasks but also to anticipate potential issues, leverage existing knowledge, and adapt its planning approach based on user-provided guidelines and project specifics.

**Role in Iterative Phased Development:**
`dev_planner` is a key agent in the "Iterative Phased Development Process," typically orchestrated by a higher-level AI such as `PromptuDev_AI`. It handles the detailed planning for each phase, acceptance analysis of implemented work, and, if necessary, the generation of remediation plans.

## 2. The Iterative Phased Development Process (Orchestrated by `PromptuDev_AI`)

This process outlines a structured approach to project execution, emphasizing iterative refinement, user involvement, and rigorous adherence to specifications.

```mermaid
graph TD
    A[User Starts PromptuDev_AI with Project Goal] --> B{Phase 0: Initialization};
    B --> C[User Provides/Confirms Inputs for dev_planner, incl. USER_PLAN_PROPOSAL_PATH (optional) & PROJECT_SPECIFICATIONS_REF];
    C --> D{User Confirms dev_planner Params & Goal Summary?};
    D -- No --> X[HALT - User Dissatisfaction];
    D -- Yes --> E{USER_PLAN_PROPOSAL_PATH Provided?};
    E -- Yes --> F[PromptuDev_AI: Process User Plan];
    F --> G[PromptuDev_AI: Independent Spec Analysis & Outline];
    G --> H{Compare User Plan & AI Outline};
    H -- Good Correlation --> I[Use User's Plan Structure];
    H -- Significant Difference --> J[PromptuDev_AI: Present Both, User Chooses Plan Structure];
    J --> I;
    H -- User Plan Issues (e.g., conflicts with Specs) --> K[PromptuDev_AI: Summarize Issues, Present AI Outline];
    K --> I;
    E -- No --> L[dev_planner: Generate Plan Outline from PROJECT_SPECIFICATIONS_REF];
    L --> M[User Approves/Refines Outline?];
    M -- No --> X;
    M -- Yes --> I;
    I --> N[Agreed Phase Structure Stored];
    N --> O{Phase 1..N: Iterative Cycle};
    O -- Start Next Phase --> P[PromptuDev_AI: Invoke dev_planner for Current Phase Detailed Planning];
    P --> Q[dev_planner: Generates Phase Plan & Task Prompts (Monolithic, Delegated, or Hybrid)];
    Q --> Q1{Critical Issues in Monolithic Planning?};
    Q1 -- Yes --> Q2[dev_planner HALTS, Reports to PromptuDev_AI];
    Q2 --> Q3[PromptuDev_AI Offers User Workflow Switch for Phase];
    Q3 --> P;
    Q1 -- No --> R[User: Executes Tasks, Merges Deliverables];
    R --> S[User: Continuation-Prompts PromptuDev_AI for Phase Review];
    S --> T[PromptuDev_AI: Invoke dev_planner for Phase Acceptance Analysis];
    T --> U[dev_planner: Produces Acceptance Analysis Report (vs. Phase Plan & Specs)];
    U --> V{All Deliverables Accepted?};
    V -- Yes --> W{All Project Phases Complete?};
    W -- Yes --> Z[Project Completion: Final Review & Consensus];
    W -- No --> O;
    V -- No --> X1[dev_planner: Creates Phase Remediation Plan];
    X1 --> P;
```

**Full Lifecycle Description:**

*   **Phase 0: Initialization & Comparative Analysis (First `PromptuDev_AI` Run)**
    1.  **`PromptuDev_AI` Introduction:** The process begins with the user invoking `PromptuDev_AI` with a high-level project goal.
    2.  **User Input Confirmation:** `PromptuDev_AI` will trigger `dev_planner`. `dev_planner`'s first mandatory step is to present all resolved input parameters (from `USER_dev_planner_CONFIG.txt` and the user's prompt) and its understanding of the `User_High_Level_Project_Goal` back to the user for confirmation. If the user disapproves of the parameters or goal summary, `dev_planner` HALTS, and `PromptuDev_AI` will require the user to correct these inputs before proceeding.
    3.  **Dual Plan Analysis (if `USER_PLAN_PROPOSAL_PATH` is provided):**
        *   `PromptuDev_AI` processes the user's proposed plan (referenced by `USER_PLAN_PROPOSAL_PATH` which is passed to `dev_planner`).
        *   Concurrently, `PromptuDev_AI` performs its own independent analysis of the documents/URLs in `PROJECT_SPECIFICATIONS_REF` to generate a high-level plan outline (e.g., key phases or epics).
        *   **Comparison & Decision:**
            *   **Good Correlation:** If the user's plan aligns well with the AI's analysis of the specifications and seems sound, `PromptuDev_AI` will proceed, using the user's plan as the primary basis for the phase structure.
            *   **Significant Difference:** If the user's plan significantly deviates from what the specifications imply, or if the AI identifies a more optimal structure, `PromptuDev_AI` will present its own generated plan outline to the user alongside a summary of the differences. The user then chooses which plan structure to adopt.
            *   **User Plan Issues:** If the user's plan has clear conflicts with the specifications or critical omissions, `PromptuDev_AI` will summarize these issues and present its own AI-generated outline as a recommended alternative.
    4.  **Outline Generation (if no `USER_PLAN_PROPOSAL_PATH`):** If `USER_PLAN_PROPOSAL_PATH` is not provided, `PromptuDev_AI` directs `dev_planner` to generate a draft plan outline based solely on `PROJECT_SPECIFICATIONS_REF`. This outline is then presented to the user for approval or refinement.
    5.  **Storing Agreed Phase Structure:** Once a plan outline (phase structure) is agreed upon, `PromptuDev_AI` stores this structure for guiding the iterative execution of subsequent phases.

*   **Phase 1...N: Iterative Planning, User Execution, AI Acceptance**
    1.  **Detailed Phase Planning:** For the current phase, `PromptuDev_AI` invokes `dev_planner`.
    2.  `dev_planner` performs detailed planning for this specific phase. This involves:
        *   Conducting focused research (potentially using `PLANNING_INPUT_CONCEPTS_DIR`).
        *   Strictly adhering to *all* documents within `PROJECT_SPECIFICATIONS_REF`.
        *   Performing conflict/ambiguity checks within and between specifications (referencing Section I.E of `dev_planner.txt`).
        *   Using `PLAN_EXEMPLARS_DIR` to guide the style and detail of its outputted plan documents.
        *   Consulting `PLAN_METHODOLOGY_GUIDES` for process or documentation guidance.
        *   This results in a detailed phase plan and granular, actionable task prompts for AI developers (or human users).
    3.  **`Monolithic` Workflow Nuance:** If `dev_planner` is operating in its `Monolithic` workflow and encounters critical issues (e.g., severe specification conflicts that even its internal review or the preliminary review in Phase 0 didn't catch, or an inability to decompose a module effectively under the given constraints), it will HALT and report these issues to `PromptuDev_AI`. `PromptuDev_AI` will then inform the user and may offer the option to switch `dev_planner` to a different workflow (e.g., `DelegatedPlanning` or `HybridIterative`) for that specific problematic phase to allow for more focused sub-planning by specialized Task AIs.
    4.  **User Task Execution:** The user (or AI agents orchestrated by the user/`PromptuDev_AI`) executes the generated tasks, creating deliverables and merging them. The session typically ends with `PromptuDev_AI` after tasks for the current phase are launched or completed by the user.
    5.  **Continuation & Phase Acceptance:** The user re-prompts `PromptuDev_AI` to continue to the next step (typically phase review/acceptance).
    6.  `PromptuDev_AI` invokes `dev_planner` again, this time in a "Phase Acceptance Analysis" mode.
    7.  `dev_planner` analyzes the merged deliverables against the original phase plan and the `PROJECT_SPECIFICATIONS_REF`. It produces an "Acceptance Analysis Report" detailing compliance, deviations, and any identified issues.
    8.  **Phase Completion Management (`PromptuDev_AI`):**
        *   **All Accepted:** If `dev_planner`'s report indicates all deliverables are accepted and meet specifications, `PromptuDev_AI` marks the phase as complete. It then either proceeds to initiate planning for the next phase or, if all phases are done, moves to Project Completion.
        *   **Not Accepted (Remediation):** If the report indicates issues, `PromptuDev_AI` instructs `dev_planner` to create a "Phase Remediation Plan." This plan will outline the necessary corrective actions, potentially generating new tasks or modifying existing ones. The process then loops back to detailed planning (`dev_planner` invocation) for these remediation items.

*   **Project Completion:**
    *   Once all phases and any necessary remediations are completed and accepted, `PromptuDev_AI` facilitates a final project review with the user to ensure consensus and formally conclude the project.

## 3. Configuring `dev_planner`: `USER_dev_planner_CONFIG.txt` Parameters

The behavior of `dev_planner` is controlled by parameters in `promptu/add_ons/dev_planner/USER_dev_planner_CONFIG.txt`. These can be overridden in the main user prompt.

*   **`PROJECT_NAME`**
    *   **Purpose:** Name of the overall project. Used for context or default output folder naming.
    *   **Format:** String (e.g., "MyWebApp")
    *   **Requirement:** Optional | **DefaultType:** Placeholder | **Default:** `MyProject`

*   **`ITERATION_ID`**
    *   **Purpose:** Identifier for the current iteration or run. Useful for tracking outputs from different planning sessions.
    *   **Format:** String (e.g., "Run_002", "Sprint3_Planning")
    *   **Requirement:** Optional | **DefaultType:** Placeholder | **Default:** `Iter_01`

*   **`WORKFLOW_TYPE`**
    *   **Purpose:** Specifies `dev_planner`'s main operational workflow.
    *   **Format:** String, one of: "Monolithic", "DelegatedPlanning", "HybridIterative"
    *   **Details:**
        *   `Monolithic`: `dev_planner` creates the full detailed plan for all modules, then (if applicable) defines all developer Task AIs. If it encounters severe issues during monolithic planning, it may HALT and (via `PromptuDev_AI`) suggest switching to a more granular workflow for the problematic parts.
        *   `DelegatedPlanning`: `dev_planner` first outlines high-level planning sub-tasks for different project modules/components, spawns "Planning Task AIs" to detail each, then awaits user review/merging of these sub-plans before proceeding to generate developer tasks.
        *   `HybridIterative`: `dev_planner` plans and potentially spawns worker AIs (planners or developers) iteratively, phase by phase or module by module, with user review points in between. This is useful for very large or evolving projects.
    *   **Requirement:** Optional | **DefaultType:** Accepted | **Default:** `Monolithic`

*   **`BASE_OUTPUT_PATH`**
    *   **Purpose:** User-defined base directory for all outputs from this specific invocation of `dev_planner`.
    *   **Format:** Repository-relative or absolute path string (e.g., `outputs/MyProject/DevPhase_Run01/`).
    *   **Note:** Structure this path carefully (e.g., two levels below `promptu/` or `promptu_dev/`) if you want shared Knowledge Base outputs from research tasks (which might use relative paths like `../../kb/`) to correctly land in the top-level `kb` folder.
    *   **Requirement:** Required | **DefaultType:** Placeholder | **Default:** `outputs/default_spawn_run/`

*   **`MAIN_DEV_PLAN_FILENAME`**
    *   **Purpose:** Filename for the main overview/manifest document that lists and links to individual submodule development plans.
    *   **Format:** String (e.g., `main_dev_plan_overview.md`)
    *   **Location:** Saved directly under `BASE_OUTPUT_PATH`.
    *   **Requirement:** Required | **DefaultType:** Accepted | **Default:** `main_development_plan.md`

*   **Output Subdirectories:**
    *   **`TASK_PROMPTS_SUBDIR`**: Subdirectory (relative to `BASE_OUTPUT_PATH`) for the Task Launch Plan (`00_task_launch_plan.md`) and individual task prompt files (`.txt`). Default: `planning_files`.
    *   **`SUBMODULE_PLANS_SUBDIR`**: Subdirectory (relative to `BASE_OUTPUT_PATH`) for detailed `_dev_plan.md` and `_next_steps.md` files generated by Planning Task AIs or Sub-Planning Task AIs. Default: `task_execution_plans`.
    *   **`TASK_ARTIFACTS_SUBDIR`**: Subdirectory (relative to `BASE_OUTPUT_PATH`) for the actual deliverables and new files created by Developer Task AIs. Default: `task_output_artifacts`.
    *   **Requirement (for all subdir params):** Required | **DefaultType:** Accepted

*   **`PLANNING_INPUT_CONCEPTS_DIR`**
    *   **Purpose:** Path to a directory containing documents, notes, or other materials that provide foundational concepts and ideas. `dev_planner` uses these as a basis for in-depth research to inform the planning phase.
    *   **Format:** Repository path string to a directory.
    *   **Requirement:** Optional | **DefaultType:** Placeholder | **Default:** `[Path to directory with input concept documents or N/A]`

*   **`PROJECT_SPECIFICATIONS_REF`**
    *   **Purpose:** Primary source of truth for project requirements. Used for architectural decisions, task breakdown, and acceptance criteria.
    *   **Format:** Comma-separated list of repository paths (to files or directories) or fully qualified URLs. If a directory is provided, `dev_planner` (and its spawned AIs) should consider all documents within.
    *   **Requirement:** Optional | **DefaultType:** Placeholder | **Default:** `[Comma-separated paths/URLs to project specifications or N/A]`

*   **`PLAN_EXEMPLARS_DIR`**
    *   **Purpose:** Path to a directory containing one or more exemplar documents (e.g., sample plan sections, technical deep-dives). These illustrate the desired level of detail, technical depth, and style for generated plan documents.
    *   **Format:** Repository path string to a directory.
    *   **Note:** While `dev_planner` uses these for style guidance, the user should ensure that the content of exemplars is consistent with project specifications. `dev_planner` itself does not perform deep conflict resolution *between* exemplars and specifications but rather uses exemplars to format its output plans.
    *   **Requirement:** Optional | **DefaultType:** Placeholder | **Default:** `[Path to directory with exemplar plan documents or N/A]`

*   **`PLAN_METHODOLOGY_GUIDES`**
    *   **Purpose:** Provides guidance on *how* specific planning or development aspects should be implemented (e.g., document management plans, change management plans, style guides, API design guides, testing processes).
    *   **Format:** Comma-separated list of repository paths (to files or directories) or fully qualified URLs.
    *   **Note:** If a directory is provided, `dev_planner` may attempt to list files within it. `dev_planner` will try to infer the purpose of each guide (e.g., from filename) and reference relevant guides in the prompts of specific developer tasks. Planning Task AIs are also instructed to review these guides.
    *   **Requirement:** Optional | **DefaultType:** Placeholder | **Default:** `[Comma-separated paths/URLs to methodology guides or N/A]`

*   **`USER_PLAN_PROPOSAL_PATH`**
    *   **Purpose:** Optional path to a single document where the user has outlined their own project plan or phase structure. Used in Phase 0 by `PromptuDev_AI` for comparative analysis.
    *   **Format:** Repository path string to a file.
    *   **Requirement:** Optional | **DefaultType:** Placeholder | **Default:** `[Path to user's plan proposal document or N/A]`

## 4. Key `dev_planner` Internal Behaviors & Protocols

(Based on `promptu/add_ons/dev_planner/dev_planner.txt`)

*   **User Confirmation of All Input Parameters:** As a first step upon invocation, `dev_planner` resolves all its configuration parameters and summarizes them along with its understanding of the `User_High_Level_Project_Goal`. It presents this to the user for explicit confirmation and will HALT if the user does not approve, preventing work based on incorrect assumptions.
*   **Preliminary Review of Project Specifications:** Before diving deep into research for its "Task 0" (Initial Research Task), `dev_planner` performs a preliminary review of `Resolved_DP_Project_Specifications_Ref` to identify any show-stopping contradictions or ambiguities that would make research unproductive. If such issues are found, it HALTS and alerts the user (as per Section I.E).
*   **Conflict and Ambiguity Resolution Protocol (Section I.E of `dev_planner.txt`):**
    *   This is a core protocol. If `dev_planner` (or a Task AI it spawns) detects critical conflicts or ambiguities in `Resolved_DP_Project_Specifications_Ref` at any planning stage, it will:
        1.  HALT the current operation.
        2.  Clearly present the conflicting items or ambiguous requirements to the user, explaining the issue and its impact.
        3.  Request specific clarification or a decision from the user.
        4.  Document the user's feedback upon receipt.
        5.  Resume or refine the planning process based on this feedback.
*   **Use of Knowledge Base for Interface-Based Design & Parallelization (referencing Section II.0.h of `dev_planner.txt`):**
    *   `dev_planner` is instructed to actively incorporate and promote interface-based design principles. This involves consulting general best practices (`promptu_dev/kb/interface_based_design_best_practices/expert_L1.md`) and specialized knowledge bases (e.g., for Rust, Java, OS-level, Kernel-level, Application Architecture partitioning) to guide the definition of clear module boundaries, robust APIs, and well-encapsulated components. This is critical for effective parallel development and minimizing merge conflicts.
*   **Acceptance Analysis (in Iterative Phases):**
    *   When invoked for "Phase Acceptance Analysis" (typically by `PromptuDev_AI`), `dev_planner` compares the merged deliverables of a phase against the original phase plan and the overarching `Resolved_DP_Project_Specifications_Ref`.
    *   It generates an "Acceptance Analysis Report" detailing compliance, deviations, and any issues found. This report informs `PromptuDev_AI`'s decision on whether the phase is complete or requires remediation.
*   **Remediation Planning:**
    *   If `PromptuDev_AI` determines (based on `dev_planner`'s acceptance report) that a phase's deliverables are not acceptable, it instructs `dev_planner` to create a "Phase Remediation Plan."
    *   This plan outlines corrective actions, which may include generating new tasks or modifying previous task definitions to address the identified shortcomings. The process then typically loops back to detailed planning and execution for these remediation items.

## 5. Using `PromptuDev_AI` with `dev_planner`

*   **Preparing Inputs:**
    *   Ensure `PROJECT_SPECIFICATIONS_REF` in `USER_dev_planner_CONFIG.txt` (or your main prompt override) points to comprehensive and clear specification documents. This is the most critical input.
    *   Optionally, provide your own initial plan via `USER_PLAN_PROPOSAL_PATH` for `PromptuDev_AI` to consider during Phase 0.
    *   Set up `PLANNING_INPUT_CONCEPTS_DIR`, `PLAN_EXEMPLARS_DIR`, and `PLAN_METHODOLOGY_GUIDES` as needed to further guide `dev_planner`.
    *   Define `PROJECT_NAME` and `BASE_OUTPUT_PATH` clearly.
*   **What to Expect:**
    *   Be prepared for an initial confirmation step from `dev_planner` regarding all its inputs and your project goal.
    *   During Phase 0, `PromptuDev_AI` will manage the establishment of an agreed-upon phase structure, potentially involving your review and choice if your proposed plan differs from its analysis or if no initial plan is provided.
    *   In subsequent phases, expect detailed plans and task prompts from `dev_planner`.
    *   Be ready to execute tasks and then re-invoke `PromptuDev_AI` for acceptance analysis.
    *   If ambiguities or conflicts are found in your specifications at any stage, `dev_planner` (or `PromptuDev_AI`) will HALT and ask for your clarification. Clear and timely responses will be crucial.
    *   The process is designed to be iterative and collaborative, with `dev_planner` providing detailed planning and analysis, and `PromptuDev_AI` orchestrating the overall lifecycle.

```
