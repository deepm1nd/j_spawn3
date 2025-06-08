# DRAFT: The 'Agentiq' Frame of Reference Concept

## 1. Overall Vision and Purpose

This document outlines the conceptual framework for 'Agentiq,' a system designed to provide an AI instance (like Jules) with a persistent, portable, and evolving **frame of reference** (FoR). The primary purpose of Agentiq is to enhance the AI's ability to:

*   Maintain continuity across sessions and tasks.
*   Adhere to established operational protocols and user preferences.
*   Execute complex, multi-step workflows reliably.
*   Engage in contextually relevant and helpful chat interactions.
*   Build, access, and utilize specialized domain knowledge effectively.
*   Evolve its understanding and capabilities over time.

Agentiq aims to be a structured yet flexible environment, primarily managed through files within a version-controlled repository (e.g., Git). It will be implemented as a Promptu application, leveraging the Promptu framework's capabilities for task management, component loading, and configuration.

## 2. Recall & Memory

This component of the FoR addresses how Agentiq maintains state, history, and continuity.

### 2.1. Session Handoff Notes
*   Utilizes the standard Promptu handoff notes mechanism (`promptu_dev/meta/handoff_notes.md` or `promptu/handoff_notes/handoff_notes.md`).
*   These notes will continue to serve as a primary mechanism for summarizing session activities, key decisions, and pending items for subsequent AI instances.
*   Agentiq may introduce conventions or templates for structuring these notes to better serve its specific recall needs.

### 2.2. Temporal Context & Continuation Prompts
*   **Concept:** Recognizing that AI interactions are sequential and history-dependent, Agentiq places significant emphasis on maintaining temporal context.
*   **Mechanism:** The 'continuation prompt' (the full prompt passed from one turn to the next, including history) is the primary vehicle for short-to-medium term temporal context.
*   **Agentiq's Role:** Agentiq will define strategies for:
    *   Optimizing the content and length of the continuation prompt.
    *   Summarizing or abstracting information from older parts of the conversation to keep the immediate context relevant and manageable.
    *   Potentially logging key conversational waypoints or decisions to a more persistent 'memory log' if they need to be recalled beyond the typical continuation prompt horizon.

### 2.3. Memory Log (Proposed)
*   A dedicated file or set of files (e.g., `promptu_dev/meta/agentiq_memory_log.md` or structured data files) could be used for long-term recall of significant events, learned preferences, or user feedback that transcends single sessions.
*   The format and update mechanisms for this log need further definition.

## 3. Rules: Operational Governance (Manifest-Based Approach)

This component defines the set of directives that govern Agentiq's behavior, adopting a flexible and modular manifest-based system as outlined in 'Proposal C' of `promptu_dev/meta/operating_rules_management_proposal.txt`.

### 3.1. Core Principle: Dynamic Activation of Directives
Instead of a monolithic block of rules, Agentiq will manage operational rules, guidelines, and preferences as individual, self-contained definition files. A 'Directives Manifest' will specify which of these individual files are loaded and considered active for the current AI session or a specific 'package' of directives.

### 3.2. Directive Libraries
*   **Structure:** Individual directive files will be stored in structured library directories. For Agentiq, these could reside within its app structure or leverage shared Promptu framework libraries if applicable:
    *   `promptu/apps/agentiq/ops/rules/`
    *   `promptu/apps/agentiq/ops/guidelines/`
    *   `promptu/apps/agentiq/ops/preferences/`
    *   Alternatively, paths like `promptu_dev/meta/ops/...` could be used if these are meant to be developer-managed for the core Agentiq. This needs to be decided during app planning (Step 4).
*   **Content:** Each file in these libraries defines a single Rule, Guideline, or Preference.
*   **File Format:** Each directive file MUST have a unique ID across the ecosystem. A suggested format is Markdown with YAML frontmatter (for ID, title, version, description) followed by the directive's content. JSON or YAML are also options.
    *Example Directive File (`example_rule.md`):*
    ```yaml
    ---
    id: agentiq_rule_001
    title: Code Commit Protocol
    version: 1.0
    description: Ensures code is linted and tested before commit.
    type: rule
    ---
    All code must be linted and pass all unit tests before being committed to the repository.
    ```

### 3.3. Directives Manifest File
*   **Purpose:** An activation list that explicitly enumerates the unique IDs of all directive files (from the libraries) that should be active for the current session or 'package.'
*   **Location (Proposed for Agentiq):** Within the Agentiq app structure, e.g., `promptu/apps/agentiq/config/directives_manifest.json`. This could also be user-configurable or project-specific.
*   **Format:** A simple list of unique directive IDs (e.g., a JSON array of strings).
    *Example `directives_manifest.json`:*
    ```json
    [
      "agentiq_rule_001",
      "agentiq_guideline_005",
      "agentiq_preference_002"
    ]
    ```

### 3.4. Agentiq's Loading and Application Logic
*   Agentiq's core components (defined as Promptu tasks) will be responsible for:
    1.  Locating and parsing the active Directives Manifest.
    2.  For each directive ID in the manifest:
        a.  Searching the configured library paths to find the corresponding directive definition file.
        b.  Loading and parsing the directive file.
        c.  Applying/enforcing it as an active directive for the session.
    3.  Handling errors (missing files, malformed files, conflicts if not designed out).

### 3.5. Benefits for Agentiq
*   **Modularity & Reusability:** Directives are individual and can be reused across different 'packages.'
*   **Customizable 'Packages':** Users or Agentiq itself can switch between different sets of active directives by changing the manifest, tailoring behavior for specific tasks (e.g., 'research mode', 'coding mode', 'planning mode').
*   **Clear Separation of Concerns:** Directive definitions are separate from Agentiq's core operational logic.

This system provides a robust foundation for managing Agentiq's operational governance in a flexible and scalable manner.

## 4. Workflows and Functional Behaviors

This component focuses on defining and managing structured sequences of actions or tasks that Agentiq can perform.

### 4.1. Task Definitions
*   Individual tasks will be defined as components within the Agentiq Promptu app (e.g., `.txt` files containing instructions for the AI).
*   Each task will have a clear purpose, inputs, outputs, and steps.

### 4.2. Workflow Orchestration
*   Complex operations will be defined as 'workflows,' which are sequences of tasks.
*   The Agentiq app's manifest (`promptu/apps/agentiq/agentiq_manifest.json`) will define phases and tasks, which can represent these workflows.
*   Agentiq may include components specifically designed for workflow management, allowing for conditional logic, looping, and error handling within a workflow.
*   Example Workflow: 'Code Review and Submission' might involve tasks like 'Fetch Changes,' 'Run Static Analysis,' 'Run Tests,' 'Format Report,' 'Await User Approval,' 'Commit Code.'

## 5. Chat Behaviors

This component provides guidelines for how Agentiq should interact with the user in chat.

### 5.1. Persona and Tone
*   Defines the desired persona (e.g., helpful assistant, expert consultant).
*   Specifies the appropriate tone of voice (e.g., formal, informal, empathetic).

### 5.2. Communication Shorthands
*   Leverages and potentially extends the shorthands defined in Promptu's `core_planning_instructions.txt` (Section V).

### 5.3. Proactive Assistance
*   Guidelines on when and how Agentiq might offer suggestions or anticipate user needs.

### 5.4. Clarification Strategies
*   Protocols for asking clarifying questions when user requests are ambiguous.

These behaviors will be documented within the Agentiq app structure (e.g., `promptu/apps/agentiq/config/chat_behaviors.md`).

## 6. Domain Knowledge

This component is critical for making Agentiq knowledgeable and insightful in specific areas. It defines how domain-specific information is captured, stored, managed, and utilized.

### 6.1. SME Leveled Expert Knowledge Files
*   **Purpose:** To store core, curated knowledge on a specific subject, organized by research depth and written for AI comprehension. This allows for progressive information access.
*   **Structure:** Within each domain-specific directory (e.g., \`promptu_dev/kb/[domain_name]/\`), expert knowledge is stored in multiple Markdown files, named according to the research level they represent:
    *   \`expert_L0.md\`: Contains a Level 0 overview, key sub-topics, and initial findings for the domain/topic.
    *   \`expert_L1.md\`: Contains a Level 1 detailed summary and synthesis.
    *   \`expert_L2.md\`: Contains a Level 2 in-depth analysis and elaboration.
    *   \`expert_L3.md\`: Contains Level 3 unique insights, critiques, or novel connections (if this level of research is performed).
*   **Content:** Each file can include textual explanations, definitions, principles, code snippets, examples, etc., appropriate to its research level.
*   **Generation:** These files are primarily generated and updated by the enhanced \`create_research_report\` add-on, corresponding to its \`REQUESTED_RESEARCH_LEVEL\` output.

### 6.2. SME Reference Files
*   **Purpose:** To store links to external web pages, documents, or other resources relevant to a domain.
*   **Structure:** Markdown files located in \`promptu_dev/kb/\` (organized by domain subdirectories, e.g., `promptu_dev/kb/[domain_name]/refs.md`).
*   **Example:** \`promptu_dev/kb/ai_agents_and_context/refs.md\`.
*   **Content:** List of URLs with brief descriptions or annotations.
*   **Targeted Selection:** To facilitate targeted research, individual references within these files may be assigned unique identifiers (e.g., \`REF_001\`) or tags. This allows for specific references to be selected or prioritized during knowledge acquisition tasks.

### 6.3. Master Domain Log
*   **Purpose:** To track all available domains and the relationships between them. This facilitates cross-domain understanding and knowledge discovery.
*   **Location:** `promptu_dev/meta/master_domain_log.md`.
*   **Structure (Proposed):**
    *   Each entry could represent a domain.
    *   Properties for each domain:
        *   `DomainID` (e.g., `ai_agents_and_context`)
        *   `DisplayName` (e.g., 'AI Agents, Swarms, and Context')
        *   `Description`
        *   `SME_Expert_File_Path`
        *   `SME_Refs_File_Path`
        *   `Related_Domains` (linking to other DomainIDs)
        *   `Tags` (for the cross-domain tagging system)

### 6.4. Cross-Domain Tagging System
*   **Purpose:** To allow flexible categorization and connection of information across different domains.
*   **Mechanism:** A controlled vocabulary or folksonomy of tags used within the `master_domain_log.md` and potentially directly within SME files.
*   **Examples:** `#agent_architecture`, `#context_management`, `#swarm_intelligence`, `#temporal_reasoning`.

### 6.5. Ongoing Refinement of File Formats
*   **Note:** The optimal file formats and structures for storing and accessing domain knowledge (especially for complex, structured data) will be a subject of ongoing review and refinement as Agentiq evolves. This includes considering trade-offs between human readability (Markdown) and machine processability (JSON, YAML, etc.) for different types of knowledge.

### 6.6. Access and Utilization
*   Agentiq components will be developed to parse, query, and synthesize information from these domain knowledge sources.
*   This might involve natural language processing techniques, keyword searching, or more structured data retrieval depending on the file formats.

### 6.6.1. Leveled Research Protocol (Operational Guideline)
To ensure interactive and controllable knowledge acquisition, Agentiq will adhere to the following 'Leveled Research Protocol' when tasked with researching a new topic or expanding existing domain knowledge:

*   **Minimal Input Acceptance:** Agentiq can initiate research based on minimal user input, such as a single keyword or a concise query.
*   **Default 'Level 0' Research:** Upon receiving a research request, Agentiq will perform an initial, broad, but relatively shallow 'Level 0' research. The goal of Level 0 is to:
    *   Define the core topic.
    *   Identify key sub-topics or facets.
    *   Locate 1-2 highly relevant overview resources.
    *   Estimate the breadth and depth of the available information.
*   **Reporting and Continuation Prompt:** After completing Level 0, Agentiq will:
    *   Summarize its findings briefly.
    *   Report that 'Level 0 research is complete.'
    *   Indicate its readiness to conduct deeper research.
    *   Prompt the user to specify a desired depth or focus for further research (e.g., 'Would you like me to proceed to Level 1 on sub-topic X, or explore another area?').
*   **User-Specified or AI-Queried Levels & Targeted References:**
    *   The user can then direct Agentiq to proceed to 'Level 1', 'Level 2', etc., potentially focusing on specific sub-topics identified in Level 0.
    *   As part of specifying research depth or focus, the user can guide Agentiq to utilize a **selectable subset of references** from one or more \`*_refs.md\` files. This selection can be guided by minimal user input (e.g., keywords matching reference descriptions, tags, or specific reference IDs).
    *   Agentiq should be capable of performing **cross-domain research** if references from multiple domain knowledge bases are indicated.
    *   If the user requests further research without specifying a level or specific references, Agentiq should ask for clarification on the desired depth, focus, or reference constraints.
*   **Definition of Levels (Iterative):** The precise definition of what each research 'level' entails (e.g., number of sources, depth of analysis, types of information extracted, use of specific references) will be an iterative refinement and may become domain-specific. This protocol itself should be considered an operational guideline for Agentiq.

This protocol will be integrated into the functionality of Agentiq components responsible for domain knowledge acquisition, such as the \`access_domain_knowledge_agentiq\` task.

## 7. Relationship to Promptu Apps and Components

Agentiq will be implemented as a **Promptu Application** named `agentiq`.

*   **Manifest (`promptu/apps/agentiq/agentiq_manifest.json`):** Will define the phases and tasks that constitute Agentiq's core functionalities (e.g., 'Initialize FoR', 'Access Domain Knowledge', 'Execute Workflow', 'Update Memory Log').
*   **Task Components (`promptu/apps/agentiq/*.txt`):** These files will contain the specific AI instructions for each task. For example, a task component might instruct the AI on how to parse the `master_domain_log.md` or how to apply a specific rule from `rules.md`.
*   **Configuration Files (`promptu/apps/agentiq/user_*.txt` and internal config):**
    *   User-facing configurations for specific Agentiq tasks.
    *   Internal configuration files storing paths to rules, chat behaviors, memory logs, etc.

The Agentiq app will essentially be the engine that brings this Frame of Reference to life, providing the AI with the tools and instructions to operate within it.

## 8. Future Considerations
*   Automated learning and knowledge base population.
*   More sophisticated memory and recall mechanisms.
*   Version control and evolution of rules and workflows.
*   Integration with external tools and APIs.

---
This draft provides a foundational structure. It will require further refinement and detail as Agentiq is developed.
