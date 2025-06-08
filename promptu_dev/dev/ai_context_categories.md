# Report: Proposed Reorganization Options for `promptu/core/core_planning_instructions.txt`

**Date:** 2024-03-15
**Prepared by:** Jules (AI Instance)
**For:** User

## 1. Introduction

This report addresses the request to review the `promptu/core/core_planning_instructions.txt` document, analyze its current structure, and propose alternative organizational options. The goal of these proposals is to enhance clarity, logical flow, and ease of reference for AI instances and human developers interacting with the Promptu framework's core instructions.

## 2. Current Document Structure Overview

The `core_planning_instructions.txt` is currently organized into four main sections:
*   **I. USER-CONFIGURED PATHS, ADD-ON SELECTION, COMPONENT DISCOVERY & LOADING**
*   **II. PRIMARY EXECUTION LOGIC (Revised for PromptApps)**
*   **III. OUTPUT GENERATION REQUIREMENTS (Core Utilities)**
*   **IV. OPERATIONAL DIRECTIVES & SELF-CORRECTION (Core)**

While functional, this structure has evolved, and opportunities exist to refine its organization for improved usability.

## 3. Proposed Organizational Options

Three distinct organizational options have been developed and are detailed below.

### 3.1. Option A: Enhanced Phased/Chronological Flow

#### 3.1.1. Rationale
This structure organizes instructions based on the typical sequence of an AI's planning and execution cycle during a session. The goal is to present information in the order it's most likely to be needed, making it intuitive for an AI instance to follow from initialization to completion of a task.

#### 3.1.2. Proposed High-Level Outline
*   **Preamble: Core Framework Principles** (Contextual AI Operation, etc.)
*   **Section 1: Session Startup & Task Definition** (Mode detection, handoff init, user request parsing)
*   **Section 2: Component Discovery, Loading & Configuration** (Paths, discovery of add-ons/utils/apps, user selections, parameter resolution)
*   **Section 3: Primary Execution Logic** (PromptApp/Add-on path determination, task execution, iteration)
*   **Section 4: Operational Governance & AI Conduct** (Rules, Guidelines, Preferences, Deviation protocols)
*   **Section 5: Output Generation & Session Finalization** (Standard outputs, commit messages, handoff notes update)
*   **Section 6: Framework Self-Improvement** (Self-correction, learning)
*   **Appendix (Optional):** (Glossary, Index)

#### 3.1.3. Pros
*   **Intuitive Flow for AI:** Follows a natural operational sequence.
*   **Good for Onboarding:** Logical for understanding the end-to-end flow.
*   **Clear Task Phases:** Sections map clearly to distinct phases of a task lifecycle.

#### 3.1.4. Cons
*   **Information Scattering:** Information about a single logical component might be split across sections.
*   **Potential Redundancy:** Some concepts might need cross-referencing.
*   **Less Ideal for Targeted Lookup:** Finding all rules on a specific topic might require checking multiple sections.

---

### 3.2. Option B: Thematic/Functional Grouping

#### 3.2.1. Rationale
This structure groups instructions based on their core function or the aspect of the Promptu framework they address, irrespective of their exact timing in a session. The goal is to create comprehensive sections for each major functional area, making it easy to find all related information on a specific topic.

#### 3.2.2. Proposed High-Level Outline
*   **Section 1: Core Framework Concepts** (Principles, Invocation Modes, Process Overview)
*   **Section 2: User & Project Configuration** (Request extraction, user selections, config blocks)
*   **Section 3: Component Lifecycle Management** (Definitions, paths, discovery, loading, parameter resolution for all component types)
*   **Section 4: Task Execution Engine** (Main execution path, PromptApp processing, Add-on processing, control flow)
*   **Section 5: AI Operational Directives (Governance)** (Rules, Guidelines, Preferences, Deviations, Self-correction)
*   **Section 6: Session Management & Reporting** (Handoff notes, standard outputs, commit messages)

#### 3.2.3. Pros
*   **Excellent for Targeted Information Retrieval:** All details of a specific function are co-located.
*   **Reduces Information Scattering.**
*   **Modular Understanding:** Promotes understanding of the framework in functional blocks.
*   **Easier Maintenance for Specific Topics.**

#### 3.2.4. Cons
*   **Less Intuitive Operational Flow:** Does not directly map to the AI's action sequence.
*   **Steeper Initial Learning Curve:** Understanding end-to-end flow requires piecing together information.

---

### 3.3. Option C: Hybrid Approach (Phased Start, then Functional Grouping)

#### 3.3.1. Rationale
This structure aims to combine the benefits of the chronological and thematic approaches. It starts with session setup in a phased manner, then groups core operational areas (like component management, execution, and governance) thematically for comprehensive coverage.

#### 3.3.2. Proposed High-Level Outline
*   **Section 1: Session Initialization & Context Setup** (Mode detection, core principles, handoff init, user request)
*   **Section 2: Component Ecosystem Management** (Functional grouping: Types, paths, discovery, loading, configuration for all components)
*   **Section 3: Primary Task Execution Logic** (Functional grouping: Main path, PromptApp lifecycle, Add-on lifecycle)
*   **Section 4: Operational Governance & Conduct** (Functional grouping: Rules, Guidelines, Preferences, Deviations, Self-correction - similar to current Section IV)
*   **Section 5: Standard Outputs & Session Finalization** (Functional grouping: Output gen, commit messages, handoff procedures)

#### 3.3.3. Pros
*   **Logical Startup:** Initial steps follow a natural chronological order.
*   **Comprehensive Thematic Sections:** Allows for deep dives into key functional areas.
*   **Balance:** Attempts to offer both a clear startup sequence and organized reference.
*   **Familiar Governance Section:** Keeps the well-understood Section IV structure thematically intact.

#### 3.3.4. Cons
*   **Transition Point:** The switch from chronological to thematic might feel slightly abrupt.
*   **Still Some Scattering:** Some low-level operations might still be separated from their initial mention.
*   **Complexity:** Might be perceived as more complex than a purely single-paradigm approach.

---

## 4. Recommendation & Next Steps

All three options offer valid ways to reorganize `core_planning_instructions.txt`, each with its own strengths and weaknesses. The best choice depends on the primary goals for the reorganization (e.g., ease of AI processing, human readability for specific queries, onboarding new developers).

It is recommended to:
1.  Review these options.
2.  Discuss which approach (or a further refinement) best meets the needs of the Promptu framework.
3.  Once a preferred structure is chosen, a subsequent task can be initiated to refactor the actual `core_planning_instructions.txt` document according to the selected option.
