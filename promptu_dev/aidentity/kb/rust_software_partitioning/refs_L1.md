# References for Best practices for software partitioning in Rust (Level 1)

Date: 2024-05-24

## Core Rust Concepts & Official Documentation (L0 & L1):

-   **[The Rust Programming Language - Ch 7: "Managing Growing Projects with Packages, Crates, and Modules"]**: [https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html](https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html)
    *   Description: Foundational for understanding modules, visibility (`pub`, `use`), paths, crate structure (`lib.rs`, `main.rs`), and packages. Sections include:
        *   Ch 7.1: "Packages and Crates" - Defines packages and crates.
        *   Ch 7.2: "Defining Modules to Control Scope and Privacy" - Explains module system, `mod`, `pub`, paths.
    *   Relevance: Primary for L0 and L1 understanding of modules, basic crate structure, and visibility.
-   **[The Rust Programming Language - Ch 14.3: "Cargo Workspaces"]**: [https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html)
    *   Description: Explains setting up and managing multi-crate projects using Cargo workspaces.
    *   Relevance: Primary L0 and L1 resource for "Workspaces for Multi-Crate Projects".
-   **[Cargo Book - Reference - Workspaces]**: [https://doc.rust-lang.org/cargo/reference/workspaces.html](https://doc.rust-lang.org/cargo/reference/workspaces.html)
    *   Description: Comprehensive details on workspace configuration, virtual manifests, member definition, and crucially, inheriting metadata and dependencies (`[workspace.package]`, `[workspace.dependencies]`).
    *   Relevance: Key L1 resource for "Workspaces for Multi-Crate Projects", providing advanced configuration details.
-   **[The Rust Programming Language - Ch 10, Sec 2: "Traits: Defining Shared Behavior"]**: [https://doc.rust-lang.org/book/ch10-02-traits.html](https://doc.rust-lang.org/book/ch10-02-traits.html)
    *   Description: Covers how traits are used to define shared functionality, acting as interfaces.
    *   Relevance: Primary L0 and L1 resource for "API Design with Traits and Encapsulation".
-   **[The Rust Programming Language - Ch 19, Sec 3: "Advanced Traits"]**: [https://doc.rust-lang.org/book/ch19-03-advanced-traits.html](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html)
    *   Description: Discusses more advanced trait concepts like associated types, default generic type parameters, supertraits, and the newtype pattern for implementing external traits on external types.
    *   Relevance: L1 resource for deeper understanding of API design with traits.
-   **[The Rust Programming Language - Ch 9: "Error Handling"]**: [https://doc.rust-lang.org/book/ch09-00-error-handling.html](https://doc.rust-lang.org/book/ch09-00-error-handling.html)
    *   Description: Details Rust's error handling mechanisms, focusing on `Result<T, E>` for recoverable errors and custom error types.
    *   Relevance: Primary L0 and L1 resource for "Error Handling for Robust Interfaces".
-   **[Rust API Guidelines - Checklist (various sections)]**: [https://rust-lang.github.io/api-guidelines/checklist.html](https://rust-lang.github.io/api-guidelines/checklist.html)
    *   Description: Community-driven guidelines for idiomatic Rust APIs. Key sections include Naming (C-CASE), Interoperability (C-COMMON-TRAITS, C-GOOD-ERR), Documentation (C-CRATE-DOC, C-HIDDEN), Type Safety (C-NEWTYPE), and Future Proofing (C-STRUCT-PRIVATE, C-SEALED).
    *   Relevance: Primary L1 resource for "API Design with Traits and Encapsulation" and "Error Handling for Robust Interfaces".
-   **[Rust Reference - Visibility and Privacy]**: [https://doc.rust-lang.org/reference/visibility-and-privacy.html](https://doc.rust-lang.org/reference/visibility-and-privacy.html)
    *   Description: Technical details from the Rust Reference on `pub`, `pub(crate)`, `pub(super)`, and `pub(in path)`.
    *   Relevance: L1 resource for deeper technical details on "Modules & Visibility".
-   **[Rust by Example - Modules (multiple sub-sections)]**: [https://doc.rust-lang.org/rust-by-example/mod.html](https://doc.rust-lang.org/rust-by-example/mod.html)
    *   Description: Practical, code-focused examples of module usage, visibility, struct visibility, `use` declarations, `super` and `self`, and file hierarchy.
    *   Relevance: Supplementary L0/L1 resource for "Modules & Visibility".
-   **[Cargo Book - Guide - Project Layout]**: [https://doc.rust-lang.org/cargo/guide/project-layout.html](https://doc.rust-lang.org/cargo/guide/project-layout.html)
    *   Description: Cargo's conventions for project structure.
    *   Relevance: Supplementary L0/L1 for "Crates & The Crate Root" and "Packages & Cargo".
-   **[Cargo Book - Reference - Manifest Format]**: [https://doc.rust-lang.org/cargo/reference/manifest.html](https://doc.rust-lang.org/cargo/reference/manifest.html)
    *   Description: Detailed specification of the `Cargo.toml` manifest file.
    *   Relevance: Supplementary L0/L1 for "Packages & Cargo".

## Additional L1 Resources (Illustrative of further research):

-   **[Effective Rust - Workspaces (Blog Post)]**: [https://www.lurklurk.org/effective-rust/workspaces.html](https://www.lurklurk.org/effective-rust/workspaces.html)
    *   Description: A blog post providing practical advice and examples for using Cargo workspaces effectively, including discussion on when to use them.
    *   Relevance: L1 supplementary material for "Workspaces for Multi-Crate Projects."
-   **[Rust Design Patterns - Newtype]**: [https://rust-unofficial.github.io/patterns/patterns/behavioural/newtype.html](https://rust-unofficial.github.io/patterns/patterns/behavioural/newtype.html)
    *   Description: Explanation and examples of the Newtype pattern for enhancing type safety in APIs.
    *   Relevance: L1 detail for "API Design with Traits and Encapsulation".
