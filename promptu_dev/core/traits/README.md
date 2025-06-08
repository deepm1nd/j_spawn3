# AIdentity Trait Library

This directory (`promptu_dev/core/traits/`) contains the central library of all individual "Traits" that define the operational behaviors, rules, guidelines, and preferences for AIdentities within the Promptu framework.

## Trait File Format

Each trait is defined in a separate `.md` file. These files use YAML frontmatter to specify metadata for the trait, including:
- `id`: A unique identifier for the trait (e.g., `Trait_Core_Rule_FileOperationsRestriction`). This ID is also used as the filename.
- `name`: A human-readable name for the trait.
- `category`: A string indicating the logical grouping of the trait (e.g., `core_rules`, `chat_behaviors`, `component_behaviors_dev_planner`).
- `description`: A brief summary of what the trait governs.
- `strictness_default`: The inherent strictness of this trait ("Rule", "Guideline", or "Preference") if not overridden in the manifest.
- `source_file`: The original document from which this trait was extracted.
- `source_section`: The specific section in the source document.
- `version`: Version of the trait definition.
- `keywords`: Relevant keywords for searching or grouping.

The body of the Markdown file contains the `**Full Directive Text:**` of the trait and optional `**Implementation Notes:**`.

## Trait Activation

Traits from this library are made active for an AIdentity session, and their operational strictness is defined, through the `active_manifest.json` file located (by default) at `promptu_dev/aidentity/directives/active_manifest.json`.

For more detailed information on the AIdentity Traits system, how to configure active traits, and how to develop new traits, please refer to the main Promptu framework documentation (../../docs/guides/aidentity_traits_usage_guide.md).
