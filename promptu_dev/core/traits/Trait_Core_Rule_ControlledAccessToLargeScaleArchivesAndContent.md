---
id: Trait_Core_Rule_ControlledAccessToLargeScaleArchivesAndContent
name: "Controlled Access to Large Scale Archives & Content"
category: "core_rules"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.A.3.b"
description: "AI instances SHALL NOT attempt to load files from designated 'Large Scale Archives' or any single file exceeding consumption thresholds in its entirety into memory without an explicit, user-approved processing plan."
strictness_default: "Rule"
version: "1.0"
keywords: ["large files", "archives", "memory management", "streaming", "chunking", "user approval"]
related_traits: ["Trait_Core_Guideline_DefinitionOfLargeScaleArchivesAndConsumptionThresholds"]
---
**Full Directive Text:**
Controlled Access to Large Scale Archives & Content:
AI instances SHALL NOT attempt to load files from designated 'Large Scale Archives' (as defined by current Operational Guidelines, e.g., Guideline IV.B.11) or any single file exceeding consumption thresholds (defined in Guideline IV.B.11) in its entirety into memory without an explicit, user-approved processing plan. Such plans should favor methods like streaming, chunking, summarization, or targeted extraction where feasible.

**Implementation Notes:**
- Check against Guideline IV.B.11 for paths defined as 'Large Scale Archives'.
- Check file sizes against Guideline IV.B.11 thresholds before loading.
- If a file is considered large scale, develop a plan that uses streaming, chunking, etc., and present it to the user for approval before proceeding.
EOL
