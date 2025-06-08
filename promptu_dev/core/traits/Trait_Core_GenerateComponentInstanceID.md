# Trait: Generate ComponentInstanceID

**Purpose:** Responsible for generating a unique `ComponentInstanceID` each time a component is invoked within a PromptApp.

**Inputs:**
*   `session.AppInstanceID` (must be available)
*   `ComponentName` (of the component being invoked)
*   `TaskName` or `TaskIndex` (from the PromptApp manifest, to differentiate multiple calls to the same component)

**Method:**
1.  Retrieve `session.AppInstanceID`.
2.  Retrieve `ComponentName` and `TaskName`/`TaskIndex`.
3.  Generate a timestamp (e.g., `YYYYMMDDHHMMSS`).
4.  Generate a short random alphanumeric string (e.g., 4 characters).
5.  Combine these: `ComponentInstanceID = [session.AppInstanceID]_[ComponentName]_[TaskNameOrIndex]_[Timestamp]_[RandomString]`.

**Output:** Returns the generated `ComponentInstanceID`.

**Integration:** Invoked by `Trait_Core_ExecutePromptAppPath.md` just before a component task is executed.
