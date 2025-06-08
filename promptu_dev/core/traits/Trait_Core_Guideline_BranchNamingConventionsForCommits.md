---
id: Trait_Core_Guideline_BranchNamingConventionsForCommits
name: "Branch Naming Conventions for Commits"
category: "core_guidelines"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.B.9"
description: "All AI commits SHALL be made to a new branch with a name following the category/type/{description} structure. Suffix for split work."
strictness_default: "Guideline"
version: "1.0"
keywords: ["git", "branch naming", "commit conventions", "version control", "workflow"]
related_traits: []
---
**Full Directive Text:**
B.9. Branch Naming Conventions for Commits
To ensure clarity and organization in the repository's version history, all commits of work performed by the AI instance SHALL be made to a new branch with an appropriate, descriptive name. Branch names MUST adhere to the following structure:

`category/type/{description}`

Where:
1.  **`category`**: A general identifier for the nature of the work. Examples include, but are not limited to:
    *   `plan`: For planning documents, proposals, or analysis work.
    *   `feature`: For new features or significant additions to functionality.
    *   `fix`: For bug fixes or corrections to existing functionality.
    *   `doc`: For additions or updates to documentation.
    *   `refactor`: For code restructuring without functional changes.
    *   `test`: For addition or modification of tests.
    *   `guideline`: For changes related to operational guidelines or rules. (Meta)

2.  **`type`**: A more specific type within the category, further detailing the work. Examples include:
    *   If `category` is `plan`: `dev-plan`, `research-proposal`, `analysis-report`, `boot-seq-docs`.
    *   If `category` is `feature`: `core-module`, `ui-component`, `api-endpoint`.
    *   If `category` is `doc`: `user-guide`, `api-reference`, `readme-update`.

3.  **`{description}`**: A short, descriptive name in `snake_case` that summarizes the specific task or content being committed. This part should be unique enough to identify the purpose of the branch.

The AI instance is responsible for constructing a compliant and informative branch name for each set of changes submitted. This initially chosen branch name is the 'base branch name'.

In the event that a task or session terminates unexpectedly, or if the AI is otherwise forced to make multiple commits for work that was intended for a single commit against the base branch name, the AI SHALL append an incrementing numerical suffix (e.g., `_0`, `_1`, `_2`) to the *entire base branch name* for each distinct commit. For example, if the base branch name was `feature/core/user-authentication`, subsequent commits for split work would be to branches like `feature/core/user-authentication_0`, `feature/core/user-authentication_1`, etc.

**Implementation Notes:**
- Before committing, construct a branch name based on the `category/type/{description}` format.
- Ensure `{description}` is in `snake_case`.
- If a previous commit for the same intended work failed or was split, append `_N` to the base branch name.
- Provide examples for common categories and types.
EOL
