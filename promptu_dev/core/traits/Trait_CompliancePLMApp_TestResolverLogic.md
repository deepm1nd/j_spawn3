---
id: Trait_CompliancePLMApp_TestResolverLogic
name: "Compliance PLM App - Test Resolver Component Logic"
category: "component_behaviors_compliance_plm_app"
source_file: "promptu_dev/components/apps/compliance_plm/test_resolver_component/test_resolver_component.txt"
source_section: "Entire File"
description: "Defines the behavior of the app-specific 'test_resolver_component' for CompliancePLM. It primarily acknowledges its execution and the value of a passed DUMMY_PARAM."
strictness_default: "Guideline"
version: "1.0"
keywords: ["compliance_plm", "app-specific component", "test component", "parameter passing"]
related_traits: []
---
**Full Directive Text:**
```
[[START OF 'test_resolver_component' (CompliancePLM APP-SPECIFIC VERSION)]]
This is the APP-SPECIFIC version of 'test_resolver_component' for CompliancePLM.
It SHOULD be executed when called from the CompliancePLM promptApp.
DUMMY_PARAM value: {{DUMMY_PARAM}}
[[END OF 'test_resolver_component' (CompliancePLM APP-SPECIFIC VERSION)]]
```

**Implementation Notes:**
- This component is a simple test utility embedded within the CompliancePLM application.
- Its primary function is to confirm that it has been invoked correctly from the CompliancePLM app.
- It's designed to receive a parameter named `DUMMY_PARAM` and acknowledge its value.
- The text "{{DUMMY_PARAM}}" suggests a placeholder that the execution framework is expected to replace with the actual value of the `DUMMY_PARAM` parameter passed to it.
EOL
