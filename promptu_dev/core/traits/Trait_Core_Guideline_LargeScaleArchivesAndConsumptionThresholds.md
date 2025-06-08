---
id: Trait_Core_Guideline_LargeScaleArchivesAndConsumptionThresholds
name: "Definition of Large Scale Archives and Consumption Thresholds"
category: "core_guidelines"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.B.11"
description: "Defines 'Large Scale Archives' paths (e.g., promptu_dev/aidentity/archive/) and consumption thresholds (5MB single file, 10MB total) requiring adherence to Rule IV.A.3.b."
strictness_default: "Guideline"
version: "1.0"
keywords: ["large files", "archives", "consumption limit", "memory management", "performance", "file size"]
related_traits: ["Trait_Core_Rule_ControlledAccessToLargeScaleArchivesAndContent"]
---
**Full Directive Text:**
11. **Definition of Large Scale Archives and Consumption Thresholds:** (Corresponds to Rule IV.A.3.b)
    i.  The following directory paths are typically designated as containing 'Large Scale Archives': \`promptu_dev/aidentity/archive/\`. Additional paths may be configured by the user or project through framework settings or project-specific guidelines if such mechanisms are implemented.
    ii. Reading a single file exceeding **5MB (five megabytes)** from any source, or reading a total combined size of more than **10MB (ten megabytes)** from multiple files (excluding those processed by an approved streaming/chunking plan) within a single primary AI task, is considered large scale consumption and requires adherence to Rule IV.A.3.b. These thresholds may be adjusted via framework settings or project-specific guidelines if such mechanisms are implemented.

**Implementation Notes:**
- This guideline provides specific paths and size thresholds for Rule IV.A.3.b.
- Before reading files, AI should estimate their size and check if the path is a designated archive.
- If thresholds are exceeded or path matches, AI must adhere to Rule IV.A.3.b (Controlled Access to Large Scale Archives), which typically involves user-approved plans for streaming or chunking.
EOL
