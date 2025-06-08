---
id: Trait_Init_DiscoverLoadPromptAppManifests
name: "Discover and Load PromptApp Manifests"
category: "initialization_config"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "I.I"
description: "Scans promptu/components/apps/ for subdirectories, reads [promptAppName]_manifest.json from each, validates structure, and stores valid manifests in Available_PromptApps_Manifest_Map."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "promptapp", "manifest", "component loading", "discovery", "validation"]
related_traits: []
---
**Full Directive Text:**
I. **Discover and Load PromptApp Manifests:**
    1.  Initialize `Available_PromptApps_Manifest_Map` (empty map for `promptAppName: manifest_object`).
    2.  Scan `promptu/components/apps/` for subdirectories. Each is a `promptAppName`.
    3.  For each `promptAppName`:
        a.  Path to manifest: `promptu/components/apps/[promptAppName]/[promptAppName]_manifest.json`.
        b.  Attempt to read and parse the JSON content of this file.
        c.  If successfully read and parsed:
            i.  Validate manifest structure (presence of `promptAppDisplayName`, `description`, `phases` array; phases have `phaseName`, `tasks` array; tasks have `taskName`, `description`, `componentName`, `isIterable`). Issue warnings for structural issues.
            ii. Store the valid manifest object in `Available_PromptApps_Manifest_Map` with `promptAppName` as key.
        d.  If manifest file is missing, unreadable, or JSON is invalid: Prepare a warning: "Warning: PromptApp '[promptAppName]' has a missing or invalid manifest at '[path_to_manifest]' and will be unavailable."

**Implementation Notes:**
- Iterates through subdirectories in `promptu/components/apps/`.
- For each, attempts to load and parse `[promptAppName]_manifest.json`.
- Validates the JSON structure for required fields (displayName, description, phases, tasks, etc.).
- Stores valid manifests in `Available_PromptApps_Manifest_Map`.
- Issues warnings for missing/invalid manifests or structural problems.
EOL
