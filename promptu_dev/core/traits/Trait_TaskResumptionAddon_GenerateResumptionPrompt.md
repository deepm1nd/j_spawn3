---
id: Trait_TaskResumptionAddon_GenerateResumptionPrompt
name: "Task Resumption Addon - Generate Resumption Prompt"
category: "component_behaviors_task_resumption_addon"
source_file: "promptu_dev/components/add_ons/task_resumption_addon/task_resumption_addon.txt"
source_section: "Steps 1-7"
description: "Defines the (deprecated) process for an AI task to generate a detailed '_resumption_prompt.txt' file. This file includes original task details, current state, work summary, and next steps, intended to allow another AI instance to resume the task."
strictness_default: "Guideline" # As it's deprecated
version: "1.0"
keywords: ["task resumption", "handoff", "state management", "prompt generation", "deprecated"]
related_traits: ["Trait_Core_AIdentity_UpdateHandoffNotes"] # Modern replacement
---
**Full Directive Text:**
**Instructions for AI (Jules) executing the main task this add-on is appended to:**

1.  **Identify Core Task Details:** Review original prompt (goal, deliverables) and `dev_plan.md` (last completed, next steps).
2.  **Summarize Work Completed:** List key actions, created/modified files, commit IDs.
3.  **Determine Output Location:** Save to 'Submodule Plan Destination' as `<OriginalTaskFilenameBase>_resumption_prompt.txt`.
4.  **Generate Resumption Prompt Content:** (Structured text with sections: Original Task Goal, Original Task Deliverables, Current Task State (last completed, next planned, work summary, files created/modified), Next Steps for Resuming AI, Original Full Prompt).
5.  **Save the Resumption Prompt.**
6.  **Update Commit Message:** Add `[TASK_RESUMPTION_PROMPT_STATUS]: Updated_At='[path_to_resumption_prompt]'` to `Notes-To-Next-Jules`.
7.  **Initial Creation Timing:** Create initial version before primary task execution, update after significant steps.

**Implementation Notes:**
- This trait describes a deprecated method for task resumption.
- The core idea was for a task AI to create a detailed text file that another AI could use to pick up the work.
- It involved summarizing the original goal, current progress, and providing the full original prompt for context.
- The new framework handoff notes (CPI IV.F and component-specific handoff files) provide a more integrated approach.
EOL
