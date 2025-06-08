# Understanding and Using the AIdentity Traits System

The AIdentity Traits system is a core component of the Promptu Framework, designed to provide a modular, configurable, and transparent way to define and manage the behavior of AI instances (AIdentities). This system replaces monolithic directive documents with a library of individual "Traits" that can be selectively activated and customized.

## What are Traits?

A Trait is a granular, self-contained definition of a specific piece of AI behavior, an operational rule, a guideline for action, or a preference for a certain approach. Each trait focuses on a single aspect of the AI's operation. For example, there are traits for:

*   **Core Operational Rules:** Fundamental directives like file operation restrictions (`Trait_Core_Rule_FileOperationsRestriction`).
*   **Operational Guidelines:** Strong recommendations such as branch naming conventions (`Trait_Core_Guideline_BranchNamingConventionsForCommits`).
*   **Operational Preferences:** Desirable but less strict behaviors, like minimizing chat verbosity (`Trait_Core_Preference_MinimizeChatFrequencyAndContent`).
*   **Chat Behaviors:** How the AI interprets user shorthands or styles its communication (`Trait_Chat_AICommStyleSessionUniqueIDsForActionDrivingQuestions`).
*   **Component-Specific Behaviors:** Rules and logic specific to how a particular add-on or app functions (e.g., `Trait_DevPlanner_MandatoryInputConfirmation`).

Traits are defined in a human-readable format, making it easier to understand the specific directives an AI is operating under.

## Why Traits?

The Trait system offers several advantages:

*   **Modularity & Granularity:** Behaviors are broken down into small, manageable units, making the AI's directive set easier to understand, modify, and maintain.
*   **Configurability:** AI behavior can be customized for different roles or tasks by selecting and activating only the relevant traits and adjusting their strictness, without altering a large, shared core instruction document.
*   **Clarity & Transparency:** Each trait file clearly defines a specific behavior, its source, and its intended purpose. This makes it easier to see exactly what rules an AI is following.
*   **Versionability:** Individual traits can be versioned, allowing for controlled evolution of AI behaviors.
*   **Reduced Redundancy:** Common behaviors can be defined once as traits and then applied or customized across different AIdentities or components.

## The Trait Library

All available trait definitions are stored as individual Markdown files within the **Trait Library**, located at:
`promptu_dev/core/traits/`

Each `.md` file in this directory represents a single trait. The filename typically matches the trait's unique `id`. The file contains:
*   **YAML Frontmatter:** Metadata such as `id`, `name`, `category`, `description`, `strictness_default` (the inherent strictness of the trait), `source_file`, `source_section`, `version`, and `keywords`.
*   **Markdown Body:** The `**Full Directive Text:**` detailing the specific instruction, and often `**Implementation Notes:**` for the AI.

## The Trait Activation Manifest

While the Trait Library contains all *available* traits, the **Trait Activation Manifest** determines which of these traits are *active* for a specific AIdentity session and what their *applied strictness* will be.

*   **Location:** The manifest is a JSON file typically located at `promptu_dev/aidentity/directives/active_manifest.json`. The exact path is specified in the AIdentity's context file (`aidentity_context.json` via the `active_directives_manifest_path` key).
*   **Purpose:** This file acts as the configuration layer that tailors the AIdentity's behavior by composing a set of active traits from the library.
*   **Structure:** The manifest has a simple JSON structure:
    ```json
    {
      "description": "Active AIdentity Directives (Traits with applied strictness)",
      "version": "1.0",
      "traits": [
        {
          "id": "Trait_Identifier_From_Library",
          "strictness": "Rule",  // "Rule", "Guideline", or "Preference"
          "enabled": true,     // true or false
          "order": 100         // Optional: for ordering/prioritization
        },
        // ... more trait entries
      ]
    }
    ```
    *   `description`: A human-readable description of this manifest.
    *   `version`: Version of the manifest format.
    *   `traits`: An array of objects, where each object configures a single trait from the library.
        *   `id`: The unique ID of the trait (must match an `id` from a file in the Trait Library).
        *   `strictness`: The operational strictness to be applied to this trait for the session. This can override the `strictness_default` defined in the trait file itself. Allowed values are "Rule", "Guideline", or "Preference".
        *   `enabled`: A boolean (`true` or `false`) indicating whether this trait should be active for the session.
        *   `order`: (Optional) An integer that can be used to determine the order of processing or display if needed.

## Customizing AIdentity Behavior

Users and developers can customize an AIdentity's behavior by modifying the `active_manifest.json` file. To change how an AI operates:

1.  **Identify Relevant Traits:** Browse the Trait Library (`promptu_dev/core/traits/`) to find traits related to the behavior you want to influence.
2.  **Modify the Manifest:**
    *   **Activate/Deactivate:** Add or remove entries from the `traits` array in `active_manifest.json`, or set the `enabled` field to `true` or `false` for an existing entry.
    *   **Change Strictness:** Modify the `strictness` field for a trait entry in the manifest to change its enforcement level (e.g., make a "Guideline" a "Rule" for a specific AIdentity setup, or downgrade a "Rule" to a "Guideline" if more flexibility is needed and understood).

The AIdentity will load this manifest at the beginning of each session (as per the "Dynamic Trait Loading and Activation Protocol" in its core planning instructions) to assemble its set of active operational directives.
