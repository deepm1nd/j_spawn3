---
id: Trait_AIdentityCfg_UseChatBehaviorProfilePath
name: "AIdentity Config - Use Chat Behavior Profile Path"
category: "chat_behaviors"
source_file: "promptu_dev/aidentity/aidentity_context.json"
source_section: "chat_behavior_profile_path"
description: "The AIdentity's chat behaviors can be further customized by a profile loaded from `chat_behavior_profile_path` (e.g., 'profiles/chat_behaviors.md') from aidentity_context.json, if the file exists."
strictness_default: "Guideline"
version: "1.0"
keywords: ["aidentity", "configuration", "chat behavior", "profile", "customization"]
related_traits: []
---
**Full Directive Text:**
`"chat_behavior_profile_path": "profiles/chat_behaviors.md"` (from `promptu_dev/aidentity/aidentity_context.json`)

**Implementation Notes:**
- If the file specified by `chat_behavior_profile_path` exists, the AI SHOULD load it.
- The content of this profile can then be used to modify or augment the AI's default chat behaviors (e.g., those defined by traits in the `chat_behaviors` category).
- This allows for easier customization of interaction style without altering core trait definitions.
- Path is relative to `aidentity_context.json`.
EOL
