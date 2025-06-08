# Developer Guide: Creating AIdentity Traits

This guide provides instructions and best practices for developers creating new AIdentity Traits for the Promptu Framework.

## Introduction to Traits

### Philosophy
AIdentity Traits are the fundamental building blocks for defining the behavior of AI instances (AIdentities) within the Promptu Framework. The core philosophy behind Traits is to move away from monolithic, hard-to-manage directive documents towards a system of:

*   **Granularity:** Each Trait governs a single, distinct aspect of AI behavior.
*   **Self-Containment:** A Trait definition includes all necessary information (metadata, full directive text, implementation notes) within its own file.
*   **Configurability:** Traits can be selectively enabled, and their operational strictness can be adjusted for different AIdentity roles or sessions via an activation manifest.

### Purpose
Traits serve several key purposes:
*   **Modular AI Design:** They allow for the construction of complex AI personalities and behavioral sets by combining smaller, reusable behavioral components.
*   **Clarity and Transparency:** The specific directives an AIdentity is operating under are explicitly defined and easily reviewable.
*   **Ease of Modification:** Changes to specific behaviors can be made by modifying individual Trait files or the activation manifest, reducing the risk of unintended side-effects associated with editing large documents.
*   **Versionability:** Individual Traits can be versioned, allowing for controlled updates and evolution of AI capabilities.

## Trait File Structure and Naming

### Location
All individual Trait definition files are stored in the Trait Library, located at:
`promptu_dev/core/traits/`

### Naming Convention
Each Trait file is named after its unique `id` (see YAML Frontmatter section below), followed by the `.md` extension.
**Format:** `[TraitID].md`
**Example:** `Trait_Core_Rule_FileOperationsRestriction.md`

### File Format
Trait files are Markdown documents (`.md`) with two main parts:
1.  **YAML Frontmatter:** A block at the beginning of the file, enclosed by triple-dashed lines (`---`), containing structured metadata about the Trait.
2.  **Markdown Body:** The main content of the Trait, including its full directive text and any implementation notes.

## YAML Frontmatter Fields

The following fields are defined for the YAML frontmatter of a Trait file. Fields marked with `(Required)` must be present.

*   **`id`**: (String, Required)
    *   A globally unique identifier for the Trait.
    *   **Convention:** `Trait_[SourceScope]_[Type]_[BriefName]`
        *   `SourceScope`: Indicates the origin or primary domain of the trait (e.g., `Core` for framework-wide, `AIdentityQB` for Agni's master prompt, `DevPlanner` for a component, `Chat`).
        *   `Type`: Indicates the default strictness or nature (e.g., `Rule`, `Guideline`, `Preference`, `Config`, `Logic`).
        *   `BriefName`: A concise, CamelCase name summarizing the trait's purpose.
    *   **Constraint:** This `id` **must** exactly match the filename (excluding the `.md` extension).
    *   **Example:** `Trait_Core_Rule_FileOperationsRestriction`

*   **`name`**: (String, Required)
    *   A human-readable name or title for the Trait. This is often used in documentation or UI.
    *   **Example:** "File Operations Restriction"

*   **`category`**: (String, Required)
    *   A string indicating the logical grouping or category of the Trait. This helps in organizing and browsing the Trait library and can be used by future tools for filtering or applying policies.
    *   **Examples:** `core_rules`, `core_guidelines`, `core_preferences`, `chat_behaviors`, `aidentity_lifecycle`, `initialization_config`, `output_formatting`, `component_behaviors_dev_planner`, `meta_rules`.
    *   Choose an existing category if applicable, or define a new one if the trait represents a new logical grouping (e.g., `component_behaviors_[ComponentName]`).

*   **`source_file`**: (String, Required)
    *   The relative path (usually from the repository root) to the original document or component file from which this Trait's logic or directive was derived.
    *   **Example:** `promptu_dev/core/core_planning_instructions.txt` or `promptu_dev/components/add_ons/dev_planner/dev_planner.txt`.

*   **`source_section`**: (String, Optional)
    *   A more specific reference within the `source_file` that points to the exact origin of the Trait's content (e.g., a section number, heading, paragraph identifier, or line numbers).
    *   **Example:** "IV.A.1" or "Phase 3: Research and Content Generation".

*   **`description`**: (String, Required)
    *   A concise, one or two-sentence summary of what the Trait governs, its purpose, and its effect on AI behavior.
    *   **Example:** "Defines restrictions on file operations: allows create/read/modify in workspace; avoids direct copy/move; emulates copy/move; requires user confirmation for deletion."

*   **`strictness_default`**: (String, Required)
    *   The inherent operational strictness of this Trait if not overridden by the `active_manifest.json`.
    *   **Allowed Values:** `"Rule"`, `"Guideline"`, `"Preference"`.
        *   `Rule`: A mandatory directive. Deviations require explicit, multi-step user approval.
        *   `Guideline`: A strong recommendation. Deviations require explicit user permission.
        *   `Preference`: A desirable behavior. Deviations require only a notification to the user.

*   **`version`**: (String, Required)
    *   The version of this specific Trait definition (e.g., "1.0", "1.0.1"). This allows for tracking changes to individual traits.

*   **`keywords`**: (List of Strings, Optional)
    *   A list of relevant keywords that can aid in searching for, categorizing, or understanding the Trait.
    *   **Example:** `["file system", "workspace", "security"]`

*   **`related_traits`**: (List of Strings, Optional)
    *   A list of `id`s of other Traits that are conceptually related to this one. This can be used to document dependencies, complementary behaviors, or hierarchical relationships.
    *   **Example:** `["Trait_Core_Guideline_FileNamingConventions"]`

## Markdown Body Content

The body of the Markdown file, appearing after the YAML frontmatter, contains the detailed definition of the Trait. It should include:

*   **`**Full Directive Text:**` (Required)
    *   This section contains the verbatim text of the directive, rule, guideline, preference, or behavioral description as it should be understood and interpreted by the AI.
    *   It should be written clearly and precisely.

*   **`**Implementation Notes:**` (Optional but Recommended)
    *   This section provides hints, clarifications, examples, or specific instructions for the AI on how to implement, interpret, or check adherence to this Trait.
    *   It can clarify ambiguities, define edge cases, or suggest specific checks the AI should perform.

**Example Markdown Body:**
```markdown
**Full Directive Text:**
File Operations Restriction:
- You ARE ALLOWED to perform create, read, and modify operations on files directly within your workspace...

**Implementation Notes:**
- When planning file manipulations, check if it's a copy or move.
- For copy: plan create new, read old, write new.
```

## Best Practices for Writing Traits

*   **Clarity and Unambiguity:** The `Full Directive Text` should be as clear, precise, and unambiguous as possible. Avoid jargon where simpler terms suffice. The AI will interpret this text literally.
*   **Atomicity:** Each Trait should ideally govern a single, distinct, and well-defined aspect of AI behavior. Avoid creating overly broad Traits that try to cover too many unrelated rules. This improves modularity and makes Traits easier to manage and configure.
*   **Actionability:** For rules and guidelines, the directive should be actionable by the AI. It should be clear what the AI must do, must not do, or should do.
*   **Testability (Conceptual):** When defining a Trait, consider how an observer (or a future automated test) might determine if the AI is adhering to it. This helps in writing clearer directives.
*   **Use Implementation Notes Wisely:** Provide `Implementation Notes` to clarify complex points, offer examples, or guide the AI on internal checks it might perform. These notes do not change the directive itself but aid in its correct application.
*   **Consistent Categorization:** Use the `category` field consistently to group related traits. Refer to existing categories before creating new ones.
*   **Meaningful IDs and Names:** Choose `id` and `name` fields that are descriptive and make the Trait's purpose easy to understand.

## Activating and Configuring Traits (`active_manifest.json`)

Simply creating a Trait file in the library does not make it active. Traits are activated and configured for a specific AIdentity session via the **Trait Activation Manifest**.

*   **Location:** `promptu_dev/aidentity/directives/active_manifest.json` (default path, can be configured in `aidentity_context.json`).
*   **Purpose:** This JSON file tells the AIdentity:
    1.  Which Traits from the library are `enabled` for the current session.
    2.  What `strictness` ("Rule", "Guideline", "Preference") should be *applied* to each enabled Trait. This can override the `strictness_default` defined in the Trait file itself.
    3.  Optionally, an `order` for processing or display.

*   **Adding a New Trait to the Manifest:** To make your newly created Trait active, you need to add an object to the `traits` array in `active_manifest.json`:
    ```json
    {
      "id": "Your_New_Trait_ID",  // Must match the id in your trait's YAML and filename
      "strictness": "Rule",       // Or "Guideline" or "Preference"
      "enabled": true,
      "order": 600                // Optional, assign a suitable order
    }
    ```

For full details on the manifest structure and how the AI loads these traits, refer to the "Dynamic Trait Loading and Activation Protocol" in `promptu_dev/core/core_planning_instructions.txt` (Section IV.A) and the "AIdentity Traits Usage Guide" (`promptu_dev/docs/guides/aidentity_traits_usage_guide.md`).

## Choosing `category` and `strictness_default`

*   **`category`**:
    *   Review existing categories in `promptu_dev/core/traits/README.md` or by browsing existing traits.
    *   If your trait governs core AI behavior applicable across many situations, consider `core_rules`, `core_guidelines`, or `core_preferences`.
    *   If it relates to how the AI initializes or manages its lifecycle, `initialization_config` or `aidentity_lifecycle` might be appropriate.
    *   For chat and communication, use `chat_behaviors`.
    *   For behaviors specific to a component (add-on or app), use `component_behaviors_[component_name]` (e.g., `component_behaviors_dev_planner`).
    *   `meta_rules` are for traits that govern how other traits or the directive system itself works.
    *   If unsure, try to find the closest existing match or discuss with the team if a new category is warranted.

*   **`strictness_default`**:
    *   **Rule:** Use for fundamental, non-negotiable directives that define core identity, safety, or critical operational protocols. Deviations should be rare and require significant user confirmation.
    *   **Guideline:** Use for strong recommendations, best practices, or standard procedures that the AI should normally follow but can deviate from with explicit user permission if a specific context warrants it.
    *   **Preference:** Use for desirable but not critical behaviors, stylistic choices, or minor operational efficiencies. The AI should try to follow them but can deviate with a simple notification if necessary.

Remember that the `strictness_default` is the *inherent* strictness. The `active_manifest.json` can override this with an `applied_strictness` for a particular AIdentity configuration.
