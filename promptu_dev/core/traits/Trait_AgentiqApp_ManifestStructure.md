---
id: Trait_AgentiqApp_ManifestStructure
name: "Agentiq App - Manifest Structure (Deprecated)"
category: "component_behaviors_agentiq_app"
source_file: "promptu_dev/components/apps/agentiq/agentiq_manifest.json"
source_section: "Full Manifest"
description: "Defines the (deprecated) Agentiq PromptApp structure with phases (Initialization, Knowledge & Context, Task & Workflow, Session Review) and associated tasks for managing an AI's Frame of Reference (FoR)."
strictness_default: "Guideline" # Deprecated app structure
version: "1.0"
keywords: ["agentiq", "promptapp", "manifest", "deprecated", "frame of reference", "workflow"]
related_traits: []
---
**Full Directive Text:**
```json
{
  "promptAppDisplayName": "Agentiq Framework Orchestrator",
  "description": "Manages the Agentiq Frame of Reference (FoR) for enhanced AI operation, including memory, rules, workflows, chat behaviors, and domain knowledge.",
  "phases": [
    {
      "phaseName": "Initialization & Configuration",
      "tasks": [
        {
          "taskName": "Initialize Agentiq Session",
          "componentName": "init_session_agentiq", ...
        },
        {
          "taskName": "Load Active Directives",
          "componentName": "load_directives_agentiq", ...
        },
        {
          "taskName": "Load Chat Protocols",
          "componentName": "load_chat_protocols_agentiq", ...
        }
      ]
    },
    {
      "phaseName": "Knowledge & Context Management",
      "tasks": [
        {
          "taskName": "Access Domain Knowledge",
          "componentName": "access_domain_knowledge_agentiq", ...
        },
        {
          "taskName": "Manage Memory Log",
          "componentName": "manage_memory_log_agentiq", ...
        }
      ]
    },
    {
      "phaseName": "Task & Workflow Execution",
      "tasks": [
        {
          "taskName": "Execute Ad-hoc Task with FoR",
          "componentName": "execute_adhoc_task_agentiq", ...
        },
        {
          "taskName": "Execute Predefined Workflow",
          "componentName": "execute_workflow_agentiq", ...
        }
      ]
    },
    {
      "phaseName": "Session Review & Evolution",
      "tasks": [
        {
          "taskName": "Agentiq Self-Reflection",
          "componentName": "self_reflection_agentiq", ...
        },
        {
          "taskName": "Prepare Handoff Notes",
          "componentName": "prepare_handoff_agentiq", ...
        }
      ]
    }
  ]
}
```

**Implementation Notes:**
- This manifest describes a (deprecated) PromptApp named "Agentiq Framework Orchestrator."
- It's structured into four phases:
    1.  Initialization & Configuration (loading session, directives, chat protocols).
    2.  Knowledge & Context Management (accessing domain knowledge, managing memory).
    3.  Task & Workflow Execution (executing ad-hoc tasks or predefined workflows).
    4.  Session Review & Evolution (self-reflection, preparing handoff).
- Each task within these phases was intended to be handled by a specific component.
- The app aimed to manage an AI's "Frame of Reference" (FoR).
EOL
