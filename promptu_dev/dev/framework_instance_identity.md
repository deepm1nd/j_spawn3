# Instance Identity Framework (V1)

## 1. Introduction

This document outlines the V1 design for the Promptu Framework's Instance Identity system. The goal is to provide AI instances (referred to as `AIdentities`) with a mechanism for maintaining continuity, evolving characteristics, and operating within a clear contextual framework. This system supports unique identification, persistent profiles, defined startup behaviors, and standardized communication protocols.

## 2. The `AIdentity` Model

An `AIdentity` represents a specific operational context or persona that a Jules instance embodies. For V1, two primary top-level `AIdentities` are defined:

*   **Developer `AIdentity`:**
    *   **ID:** `Agni`
    *   **File/Path Prefix:** `agni`
    *   **Associated Root:** `/promptu_dev/`
    *   **Purpose:** Used for development-mode operations, framework enhancements, and tasks requiring access to developer resources.
*   **User `AIdentity`:**
    *   **ID:** `Kavia`
    *   **File/Path Prefix:** `kavia`
    *   **Associated Root:** `/promptu/`
    *   **Purpose:** Used for application-mode operations, executing user-facing Promptu Apps, and general user assistance.

Each `AIdentity` possesses a persistent profile to store its evolving characteristics.

## 3. `aidentity_context.json` - The AIdentity Profile

This JSON file stores the persistent characteristics for a specific `AIdentity`. It is key to enabling session continuity and `AIdentity` evolution.

*   **Location:** `<AIdentity_Root_Path>/<aidentity_id_filename_prefix>_aidentity_context.json`
    *   Example for Developer: `/promptu_dev/agni_aidentity_context.json`
    *   Example for User: `/promptu/kavia_aidentity_context.json`
*   **V1 Fields (snake_case for keys):**
    1.  `aidentity_id` (string): The unique ID of this `AIdentity` (e.g., "Agni", "Kavia").
    2.  `master_prompt_path` (string): Path to the core operational prompt file defining the `AIdentity`'s current primary role or behavior (e.g., `/promptu_dev/master_prompts/PromptuDev_AI_Orchestrator_v1.txt`). This prompt initiates the `AIdentity`'s startup workflow.
    3.  `active_directives_manifest_path` (string): Path to a JSON manifest file. This manifest lists the unique IDs of individual rule, guideline, and preference files (from shared libraries, e.g., `promptu/core/ops/`) that are currently active for this `AIdentity`, enabling modular and dynamic operational governance (aligns with Frame of Reference concept).
    4.  `session_log` (list of objects): Provides a structured history. Each object represents a past session and contains:
        *   `session_id` (string, unique)
        *   `start_timestamp` (ISO8601 string)
        *   `end_timestamp` (ISO8601 string)
        *   `primary_directive_for_session` (string, usually from consumed handoff)
        *   `key_outcomes_decisions` (list of strings or structured objects)
        *   `path_to_archived_handoff_note_generated` (string, path to the handoff note it produced)
        *   `path_to_consumed_stale_handoff_note` (string, path to the handoff note it consumed at start)
    5.  `memory_log_path` (string): Path to a dedicated log file (e.g., Markdown or structured text) for significant long-term memories, critical user feedback, pivotal learnings, and other persistent information that doesn't fit neatly into session summaries or structured rules (aligns with Frame of Reference concept).
    6.  `knowledge_management_profile` (object): Tracks interaction with the shared Knowledge Base.
        *   `kb_contributions` (list of objects): Details KB artifacts created/updated by this `AIdentity` (each object: `artifact_path`, `contribution_type` [e.g., "created_expert_L1"], `timestamp`).
        *   `master_domain_log_awareness` (string): Version identifier or last-checked timestamp of the `master_domain_log.md` it is familiar with.
    7.  `workflow_execution_state` (object): Stores state for resumable complex workflows (e.g., `current_workflow_id`, `current_task_id_within_workflow`, `task_specific_state_variables_map`). This is crucial for allowing an `AIdentity` to pick up multi-step tasks across sessions.
    8.  `chat_behavior_profile_path` (string): Path to a document defining this `AIdentity`'s specific chat persona, tone, extended communication shorthands, and clarification strategies (aligns with Frame of Reference concept).
    9.  `framework_context` (object): Information about the operational environment.
        *   `version_core_instructions_active` (string): Version/hash of `promptu/core/core_planning_instructions.txt`.
        *   `key_app_versions_active` (list of objects): Versions of primary Promptu Apps it's designed to work with (e.g., `{"app_name": "dev_planner", "version": "v0.2"}`).
        *   `key_addon_versions_active` (list of objects): Versions of critical add-ons.
    10. `user_todo_paths` (list of strings): Paths to files storing structured Todo items (see Section 5.1).
    11. `user_tickler_paths` (list of strings): Paths to files storing structured Tickler items (see Section 5.2).


## 4. `AIdentity` Startup Workflow

This workflow describes how an `AIdentity` (especially an orchestrator type like `PromptuDev_AI`) initializes itself for a new session. This sequence is typically defined in its `master_prompt_path` file.

1.  **Read `aidentity_context.json`:** Load its own persistent profile.
2.  **Process Core Planning Instructions:** Load and understand `promptu/core/core_planning_instructions.txt` (or its specified version, path often from `[[PROCESS...]]` directive in master prompt).
3.  **Activate User Directives:** Read the manifest specified by `active_directives_manifest_path` and load all specified rules, guidelines, and preferences, making them part of its operational context for the session.
4.  **Load & Prioritize Handoff Notes:** Implement the "Initial Action Protocol":
    *   Access `stale_notes_content` (from previous session's `handoff_notes.md`, provided by the framework).
    *   Parse the "Focus for Next Jules Instance" section. This becomes the primary directive for the current session, overriding more generic goals if specified.
5.  **Bootstrap KB Knowledge:**
    *   Access the `master_domain_log.md` (path potentially from `knowledge_management_profile.master_domain_log_awareness` or a fixed location).
    *   Identify all known KB domains.
    *   For each domain, read its `expert_L0.md` file to gain a broad, foundational understanding (respecting any content consumption limits).
6.  **Discover Framework Components:** Perform a scan (as per `core_planning_instructions.txt` Section I) to identify available Promptu Apps, Add-ons, and Utils. (For V1, this is a fresh scan; a cache mechanism is a future consideration).
7.  **Process Todos & Ticklers:**
    *   Read structured Todo items from file(s) in `user_todo_paths`.
    *   Read structured Tickler items from file(s) in `user_tickler_paths`.
    *   Identify any Ticklers that are due or upcoming.
8.  **User Interaction: Welcome & Initial Summary:**
    *   Greet the user.
    *   Summarize high-priority or new Todo items.
    *   Summarize due/upcoming Ticklers (and any Todos they link to).
    *   Summarize any open/pending tasks or specific notes flagged for follow-up from the consumed `stale_notes_content` (previous session's handoff).
9.  **Proceed with Primary Directive:** Begin executing the primary directive identified from the handoff notes, using its full operational context.

## 5. Todo and Tickler Systems (V1)

These systems help the `AIdentity` manage tasks, reminders, and user requests.

### 5.1. Todo System
*   **Purpose:** Track actionable items, user requests, or self-assigned tasks.
*   **Storage:** Stored in files listed in `aidentity_context.json:user_todo_paths`. Format per file could be a list of JSON objects.
*   **V1 Todo Item Structure:**
    *   `todo_id` (string, unique within its list/file)
    *   `description` (string)
    *   `priority` (string, e.g., "High", "Medium", "Low")
    *   `status` (string, e.g., "Open", "InProgress", "Done", "Blocked", "Idea", "Formalized")
    *   `complexity` (string, e.g., "S", "M", "L", "XL")
    *   `created_ts` (timestamp string)
    *   `due_ts` (optional timestamp string, for non-recurring due dates)
    *   `related_todo_ids` (optional list of strings, linking to other `todo_id`s for sub-tasks or dependencies)

### 5.2. Tickler System
*   **Purpose:** Time-based reminders and recurring tasks.
*   **Storage:** Stored in files listed in `aidentity_context.json:user_tickler_paths`.
*   **V1 Tickler Item Structure:**
    *   `tickler_id` (string, unique)
    *   `description` (string): The Tickler's own note/summary. Can exist even if linking to a todo.
    *   `recurrence_type` (string: "Hourly", "Daily", "Weekly", "Monthly", "Yearly", "Once")
    *   `recurrence_detail` (string): Specifics for the type (e.g., for Weekly: "Monday@09:00"; for Monthly: "15th"; for Yearly: "MM-DD"; for Hourly: ":00"; for "Once": "YYYY-MM-DD HH:MM").
    *   `next_due_ts` (timestamp string): Calculated next due date/time. The AI is responsible for updating this after a tickler fires and is processed.
    *   `status` (string: "Active", "Snoozed", "Completed", "Overdue")
    *   `created_ts` (timestamp string)
    *   `linked_todo_id` (optional string): The `todo_id` this tickler is primarily associated with, if any.

*   **Relationship:** Ticklers can optionally link to a single Todo item. This provides a reminder mechanism for more detailed tasks residing in the Todo system. Todos do not link to Ticklers.

## 6. Communication Protocols & Guidelines

To ensure efficient and clear interaction, `AIdentities` will adhere to specific communication protocols. These will be formally documented in separate guideline/rule files within `promptu/core/ops/` and activated via the `active_directives_manifest_path`.

### 6.1. Q&A Style (Ref: `promptu/core/ops/guidelines/q_and_a_style.md`)
*   **AI Question Structure:** When an `AIdentity` asks the user questions, it must frame them to allow for:
    1.  A simple Yes/No (`y`/`n`) answer.
    2.  An answer where the user can copy/paste from options provided by the AI.
    3.  If a question requires a more complex answer, the AI should understand this implies it should pause further action on that specific topic until more discussion clarifies the matter.
*   **Message ID Format:** All AI messages containing questions or requiring user input must start with a unique Message ID in the format: `[AIDENTITY_ID]_QUERY_[SESSION_INCREMENTING_SEQ_NUM]`.
    *   `[AIDENTITY_ID]`: The `aidentity_id` of the currently operating `AIdentity` (e.g., `Agni`, `Kavia`). This prefix is stable for that `AIdentity` across all its sessions.
    *   `[SESSION_INCREMENTING_SEQ_NUM]`: A counter (e.g., `001`, `002`) that increments for each query within the current session and resets at the start of a new session.

### 6.2. Action Notice and Approval Protocol (Ref: `promptu/core/ops/rules/action_approval_protocol.md`)
*   **ACTION NOTICE Requirement:** The `AIdentity` must issue an "ACTION NOTICE" and await explicit user approval before:
    1.  Starting any significant work ("MAJOR ACTION NOTICE").
    2.  Preparing files for a commit ("COMMIT ACTION NOTICE").
*   **Notice Structure:**
    ```
    JULES_ACTION_NOTICE_REF_[M|C][SEQ_NUM]
    Type: [MAJOR|COMMIT] ACTION NOTICE
    Approval Code: [RANDOM_4_DIGIT_NUM]
    Summary: [Details of action AI will take]
    To approve, respond with: APPROVE FINAL ACTION NOTICE [TYPE]-[RANDOM_4_DIGIT_NUM]
    ```
    *   `[TYPE_CODE]` is `M` for MAJOR, `C` for COMMIT.
    *   `[SEQ_NUM]` is an incrementing counter for notices issued by the AI.
    *   `[RANDOM_4_DIGIT_NUM]` is a 4-digit random number generated by the AI for that notice.
*   **User Approval:** User must reply with the exact passphrase, including the correct type and random number.

### 6.3. User-Initiated ACK Protocol (Ref: `promptu/core/ops/rules/ack_protocol_rule.md`)
*   **User Trigger:** User states in chat: `ACK '[passphrase_or_test_message]'`.
*   **AI Response:** The `AIdentity` MUST reply with the exact `[passphrase_or_test_message]` as soon as possible, without stopping or restarting any current work (no penalty).
*   **Purpose:** Allows user to verify message receipt and synchronization.

## 7. Reasoning for Key Design Decisions

*   **Top-Level AIdentities:** Simplifies profile management compared to per-instance-run profiles, allowing `AIdentities` to learn and evolve consistently over time based on the mode of operation (Developer vs. User).
*   **`aidentity_context.json`:** Provides a single, structured file for an `AIdentity`'s core persistent characteristics, making it easier to manage, backup, and potentially transfer.
*   **Manifest-Based Directives:** Offers flexibility and modularity in defining an `AIdentity`'s operational rules, allowing for easier updates and different "sets" of active directives for various roles or tasks without altering core AI code.
*   **Explicit Startup Workflow:** Ensures consistent initialization and context loading for every session, improving reliability and predictability.
*   **Structured Todos/Ticklers:** Moves beyond simple lists to more manageable and actionable items. Separation allows focused handling of tasks vs. time-based reminders, with optional linking from Ticklers to Todos.
*   **Standardized Communication Protocols:** Aims to reduce misunderstandings and improve the efficiency of AI-user interaction. The ACK protocol specifically addresses potential synchronization concerns. The Action Notice protocol ensures user oversight for significant operations.

This document provides the V1 foundation for the Instance Identity Framework. Further refinements and features are expected as the Promptu system evolves.
