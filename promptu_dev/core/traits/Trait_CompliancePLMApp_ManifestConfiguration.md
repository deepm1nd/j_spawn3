---
id: Trait_CompliancePLMApp_ManifestConfiguration
name: "Compliance PLM App - Manifest Configuration"
category: "component_behaviors_compliance_plm_app"
source_file: "promptu_dev/components/apps/compliance_plm/compliance_plm_manifest.json"
source_section: "Entire Manifest"
description: "Defines the manifest structure for the Compliance PLM app, including its phases (Concept, Plan, Development, Release, Retire) and tasks. Tasks predominantly use 'create_research_report' for documentation in Concept Phase and 'dev_planner' for planning in Plan and Development Phases, with specific defaultConfigOverrides."
strictness_default: "Rule"
version: "1.0"
keywords: ["compliance_plm", "app", "manifest", "dev_planner", "create_research_report", "configuration"]
related_traits: ["Trait_DevPlanner_ParameterResolutionOrder", "Trait_CreateResearchReport_ParameterDefinitionAndResolution"]
---
**Full Directive Text:**
```json
{
  "promptAppDisplayName": "Compliance PLM Workflow",
  "description": "A multi-phase workflow for managing Product Lifecycle Management with a focus on compliance.",
  "phases": [
    {
      "phaseName": "Concept Phase",
      "tasks": [
        // Uses create_research_report for Research, Arch Spec, Prod Spec, Prod Reqs
        // Example:
        // {
        //   "taskName": "Architectural Specification",
        //   "componentName": "create_research_report",
        //   "defaultConfigOverrides": { "REPORT_BASENAME": "Architectural_Specification" }
        // }
      ]
    },
    {
      "phaseName": "Plan Phase",
      "tasks": [
        // Uses dev_planner for Development, Change Mgmt, Doc Mgmt, Reqs Mgmt, Safety, CyberSecurity Planning
        // Example:
        // {
        //   "taskName": "Development Planning",
        //   "componentName": "dev_planner",
        //   "defaultConfigOverrides": {
        //     "PROJECT_NAME": "CompliancePLM_Development",
        //     "MAIN_DEV_PLAN_FILENAME": "Development_Overall_Plan.md",
        //     "WORKFLOW_TYPE": "Monolithic", ...
        //   }
        // }
      ]
    },
    {
      "phaseName": "Development Phase",
      "tasks": [
        // Uses dev_planner for Design, Develop, Test, Review
        // Example:
        // {
        //   "taskName": "Design",
        //   "componentName": "dev_planner",
        //   "defaultConfigOverrides": { "PROJECT_NAME": "CompliancePLM_Dev_Design", ...}
        // }
      ]
    },
    { "phaseName": "Release Phase", "tasks": [] },
    { "phaseName": "Retire Phase", "tasks": [] }
  ]
}
```
**(Note: Task arrays in the original manifest are more populated; above is a structural summary highlighting component usage and overrides.)**

**Implementation Notes:**
- This manifest defines a multi-phase (Concept, Plan, Development, Release, Retire) workflow for Compliance PLM.
- **Concept Phase:** Primarily uses `create_research_report` for generating various specification and requirements documents, overriding `REPORT_BASENAME` for each.
- **Plan Phase:** Uses `dev_planner` for creating different types of plans (Development, Change Management, etc.). It provides specific `PROJECT_NAME`, `MAIN_DEV_PLAN_FILENAME`, and other planning parameters as `defaultConfigOverrides` for each `dev_planner` invocation.
- **Development Phase:** Also uses `dev_planner` for Design, Develop, Test, and Review sub-phases, again with specific `PROJECT_NAME` overrides.
- Release and Retire phases are defined but have no tasks in this manifest version.
- This demonstrates how a PromptApp can orchestrate and customize reusable components like `dev_planner` and `create_research_report` for different purposes within its lifecycle.
EOL
