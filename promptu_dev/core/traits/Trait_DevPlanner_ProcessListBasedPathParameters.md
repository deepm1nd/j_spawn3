---
id: Trait_DevPlanner_ProcessListBasedPathParameters
name: "DevPlanner - Process List-Based Path Parameters (Methodology Guides & Exemplars)"
category: "component_behaviors_dev_planner"
source_file: "promptu_dev/components/add_ons/dev_planner/dev_planner.txt"
source_section: "I.G"
description: "Defines how dev_planner processes Resolved_DP_Plan_Methodology_Guides (expanding directory paths to file lists) and Resolved_DP_Plan_Exemplar_Refs (performing a consistency/conflict check if multiple are provided)."
strictness_default: "Rule"
version: "1.0"
keywords: ["dev_planner", "configuration", "parameters", "file paths", "methodology guides", "exemplars", "list processing"]
related_traits: ["Trait_DevPlanner_ParameterResolutionOrder", "Trait_DevPlanner_ConflictAndAmbiguityResolutionProtocol"]
---
**Full Directive Text:**
G.  **Processing List-Based Path Parameters (Methodology Guides & Exemplars):**
    1.  **Methodology Guides (`Resolved_DP_Plan_Methodology_Guides`):**
        a.  If this parameter resolves to a list of paths/URLs: For each item in the list that is a repository directory path, you MUST traverse that directory (recursively, if subdirectories are present) and expand it into a list of all relevant individual file paths (e.g., `.md`, `.txt`, `.pdf`). URLs pointing to specific files remain as is.
        b.  The final `Resolved_DP_Plan_Methodology_Guides` internal variable should then be an updated list containing only full file paths and/or URLs to specific documents. This expanded list is what will be used when these guides are referenced later.
    2.  **Plan Exemplars (`Resolved_DP_Plan_Exemplar_Refs`):**
        a.  This parameter is expected to be a list of specific file paths or URLs after initial resolution (as per Section I.B note).
        b.  **Consistency/Conflict Check:** If multiple exemplars are provided in `Resolved_DP_Plan_Exemplar_Refs`, perform a high-level review of their content. If you find exemplars that provide directly contradictory structural or stylistic instructions for similar planning elements (e.g., one exemplar mandates Mermaid diagrams for all flows, another explicitly forbids them for similar contexts), and this would make it impossible to apply them consistently, you MUST use the Conflict and Ambiguity Resolution Protocol (Section I.E) to query the user for clarification on which exemplar to prioritize or how to resolve the conflict. This check should occur before these exemplars are heavily relied upon in detailed plan document generation. If no direct conflicts in instruction are found, proceed. Minor stylistic variations are acceptable.

**Implementation Notes:**
- For `Resolved_DP_Plan_Methodology_Guides`:
    - If paths are directories, recursively expand them to individual file paths.
    - URLs to specific files remain as is.
    - The result is a flat list of file paths/URLs.
- For `Resolved_DP_Plan_Exemplar_Refs`:
    - Expected to be a list of file paths/URLs.
    - If multiple exemplars, perform a high-level consistency check.
    - If direct contradictions are found, use the Conflict and Ambiguity Resolution Protocol (I.E) to ask the user for clarification.
EOL
