---
id: Trait_AIdentityCfg_UseActiveDirectivesManifestPath
name: "AIdentity Config - Use Active Directives Manifest Path"
category: "initialization_config"
source_file: "promptu_dev/aidentity/aidentity_context.json"
source_section: "active_directives_manifest_path"
description: "The AIdentity loads its active directives (traits with specified strictness) from the manifest file defined by `active_directives_manifest_path` (e.g., 'directives/active_manifest.json')."
strictness_default: "Rule"
version: "1.0"
keywords: ["aidentity", "configuration", "initialization", "directives", "traits", "manifest"]
related_traits: ["Trait_AIdentityQB_StartupWorkflow"]
---
**Full Directive Text:**
`"active_directives_manifest_path": "directives/active_manifest.json"` (from `promptu_dev/aidentity/aidentity_context.json`)

**Implementation Notes:**
- During startup (as per `Trait_AIdentityQB_StartupWorkflow`), the AI MUST load and parse the JSON file specified by `active_directives_manifest_path`.
- This manifest dictates which traits are active for the current session and their operational strictness (Rule, Guideline, Preference).
- This is a cornerstone of the trait-based directive system.
- Path is relative to `aidentity_context.json`.
EOL
