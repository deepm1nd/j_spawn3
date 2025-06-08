# References for Best practices for software partitioning in Rust (Level 0)

Date: 2024-05-24

## Core Rust Concepts (Book & Cargo Reference):

-   **[The Rust Programming Language - Ch 7, Sec 2: "Defining Modules to Control Scope and Privacy"]**: [https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html)
    *   Description: Covers modules, `pub` for visibility, `use` for bringing paths into scope.
    *   Relevance: Primary resource for "Modules & Visibility" subtopic.
-   **[Rust By Example - Modules]**: [https://doc.rust-lang.org/rust-by-example/mod.html](https://doc.rust-lang.org/rust-by-example/mod.html)
    *   Description: Practical examples of module usage and visibility rules.
    *   Relevance: Supplementary resource for "Modules & Visibility".
-   **[The Rust Programming Language - Ch 7, Sec 1: "Packages and Crates"]**: [https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html)
    *   Description: Defines packages and crates, explaining the role of `lib.rs` and `main.rs` as crate roots.
    *   Relevance: Primary resource for "Crates & The Crate Root" subtopic.
-   **[Cargo Book - Project Layout]**: [https://doc.rust-lang.org/cargo/guide/project-layout.html](https://doc.rust-lang.org/cargo/guide/project-layout.html)
    *   Description: Official Cargo documentation on conventional project structure.
    *   Relevance: Supplementary resource for "Crates & The Crate Root".
-   **[The Rust Programming Language - Ch 1, Sec 3: "Hello, Cargo!"]**: [https://doc.rust-lang.org/book/ch01-03-hello-cargo.html](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
    *   Description: Introduces Cargo and its role in managing Rust packages and building crates.
    *   Relevance: Primary resource for "Packages & Cargo" subtopic.
-   **[Cargo Book - Manifest Format]**: [https://doc.rust-lang.org/cargo/reference/manifest.html](https://doc.rust-lang.org/cargo/reference/manifest.html)
    *   Description: Detailed specification of the `Cargo.toml` manifest file.
    *   Relevance: Supplementary resource for "Packages & Cargo".
-   **[The Rust Programming Language - Ch 14, Sec 3: "Cargo Workspaces"]**: [https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html)
    *   Description: Explains how to manage multiple related crates within a single workspace.
    *   Relevance: Primary resource for "Workspaces for Multi-Crate Projects" subtopic.
-   **[Cargo Book - Workspaces]**: [https://doc.rust-lang.org/cargo/reference/workspaces.html](https://doc.rust-lang.org/cargo/reference/workspaces.html)
    *   Description: Detailed reference on workspace configuration, including virtual manifests and inheriting dependencies/metadata.
    *   Relevance: Key supplementary resource for "Workspaces for Multi-Crate Projects".
-   **[The Rust Programming Language - Ch 10, Sec 2: "Traits: Defining Shared Behavior"]**: [https://doc.rust-lang.org/book/ch10-02-traits.html](https://doc.rust-lang.org/book/ch10-02-traits.html)
    *   Description: Covers how traits are used to define shared functionality, acting as interfaces.
    *   Relevance: Primary resource for "API Design with Traits and Encapsulation".
-   **[Rust API Guidelines - Future Proofing (C-STRUCT-PRIVATE)]**: [https://rust-lang.github.io/api-guidelines/future-proofing.html#c-struct-private-structs-have-private-fields-c-struct-private](https://rust-lang.github.io/api-guidelines/future-proofing.html#c-struct-private-structs-have-private-fields-c-struct-private)
    *   Description: Guideline on keeping struct fields private for better API stability and encapsulation.
    *   Relevance: Supplementary resource for "API Design with Traits and Encapsulation".
-   **[The Rust Programming Language - Ch 9: "Error Handling"]**: [https://doc.rust-lang.org/book/ch09-00-error-handling.html](https://doc.rust-lang.org/book/ch09-00-error-handling.html)
    *   Description: Details Rust's error handling mechanisms, focusing on `Result<T, E>` for recoverable errors.
    *   Relevance: Primary resource for "Error Handling for Robust Interfaces".
-   **[Rust API Guidelines - Error Types (C-GOOD-ERR)]**: [https://rust-lang.github.io/api-guidelines/interoperability.html#c-good-err-error-types-are-meaningful-and-well-behaved-c-good-err](https://rust-lang.github.io/api-guidelines/interoperability.html#c-good-err-error-types-are-meaningful-and-well-behaved-c-good-err)
    *   Description: Guidelines for designing effective error types for public APIs.
    *   Relevance: Supplementary resource for "Error Handling for Robust Interfaces".
