# Trait: Generate AppInstanceID

**Purpose:** Responsible for generating a unique `AppInstanceID` when a PromptApp session is initiated.

**Method:**
1.  Retrieve the `AppName` (e.g., from a session parameter or PromptApp manifest).
2.  Generate a timestamp (e.g., `YYYYMMDDHHMMSS`).
3.  Generate a short random alphanumeric string (e.g., 6 characters).
4.  Combine these: `AppInstanceID = [AppName]_[Timestamp]_[RandomString]`.
5.  Store the generated `AppInstanceID` in a persistent session variable (e.g., `session.AppInstanceID`).

**Output:** Sets `session.AppInstanceID`.

**Integration:** Invoked by `core_planning_instructions.txt` (Sec I.J - Initialization) when Aidentity starts processing a PromptApp.
