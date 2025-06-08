---
id: Trait_Init_UseFixedGlobalSystemPaths
name: "Use Fixed Global System Paths for Core Components"
category: "initialization_config"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "I.E"
description: "Core framework components (PromptApps, Add-ons, Utils, Base IEP, IPC directory) MUST be located using specified hardcoded paths relative to the repository root."
strictness_default: "Rule"
version: "1.0"
keywords: ["initialization", "file paths", "system paths", "core components", "configuration"]
related_traits: []
---
**Full Directive Text:**
E. **Fixed Global System Paths for Core Components:**
    The locations for core framework components (PromptApps, Add-ons, Utils, Base IEP, IPC directory) are fixed and relative to the repository root. You **must** use these hardcoded paths for discovery and loading:
    *   **PromptApps Base Directory:** `promptu/components/apps/`
    *   **Add-ons Base Directory:** `promptu/components/add_ons/`
    *   **Utils Base Directory:** `promptu/components/utils/`
    *   **IEP Base Directory:** `promptu/core/` (specifically for `base_iep.txt`)
    *   **IPC Base Directory:** `promptu/ipc/` (for future IPC components and general IPC file storage)

**Implementation Notes:**
- When the AI needs to find or load core components, it must use these exact relative paths.
- This ensures stability and predictability in locating essential framework parts.
- No dynamic path discovery should be used for these specific directories.
EOL
