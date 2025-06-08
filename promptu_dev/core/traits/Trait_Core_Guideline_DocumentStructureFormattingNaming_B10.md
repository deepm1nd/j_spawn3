---
id: Trait_Core_Guideline_DocumentStructureFormattingNaming_B10
name: "Document Structure, Formatting, and Naming Conventions (B.10)"
category: "core_guidelines"
source_file: "promptu_dev/core/core_planning_instructions.txt"
source_section: "IV.B.10"
description: "AI-generated documents SHALL use numerical multilevel headings, descriptive naming/titling, and formatting for clarity."
strictness_default: "Guideline"
version: "1.0"
keywords: ["document structure", "headings", "titles", "filenames", "formatting", "naming conventions", "clarity", "readability"]
related_traits: ["Trait_Core_Guideline_DocumentFormattingAndNamingConventions_B7", "Trait_Core_Guideline_FileNamingConventionsSnakeCase"]
---
**Full Directive Text:**
B.10. Document Structure, Formatting, and Naming Conventions

When constructing textual documents (e.g., proposals, plans, reports, analyses, or other significant written artifacts intended for user review or as persistent documentation within the framework), the AI instance SHALL adhere to the following conventions:

1.  **Multilevel Heading Structure:**
    *   Employ a numerical multilevel heading structure to organize content logically and hierarchically where the document's complexity warrants it.
    *   The preferred heading format is:
        *   `1. Main Section`
        *   `1.1. Subsection`
        *   `1.1.1. Sub-subsection`
        *   And so on, as needed.
    *   Ensure headings are consistently applied.

2.  **Descriptive Naming and Titling:**
    *   Strive to use clear, descriptive, and intuitive titles for documents themselves.
    *   Section headings and subheadings within documents should accurately reflect the content of the section and be easy to understand.
    *   Filenames for these documents should also be descriptive, typically using `snake_case` as per Guideline B.3 (File Naming Conventions), and should clearly indicate the document's purpose or primary subject.

3.  **Clarity and Readability:**
    *   Organize information logically within sections.
    *   Use formatting (e.g., bullet points, bolding for emphasis on key terms or actions where appropriate, code blocks for code/paths) to enhance readability and scannability.
    *   *(Placeholder for other potential clarity/readability guidelines, e.g., concise language, defining acronyms.)*

**Implementation Notes:**
- Reinforces and expands on IV.B.7.
- Emphasizes numerical multilevel headings (1., 1.1, 1.1.1).
- Links filename conventions to Guideline B.3 (snake_case).
- Highlights formatting for readability (bullets, bolding, code blocks).
EOL
