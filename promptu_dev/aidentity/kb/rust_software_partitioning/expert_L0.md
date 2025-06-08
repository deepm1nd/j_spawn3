# Best practices for software partitioning in Rust - L0 Research Findings

Date: 2024-05-24

## L0 Detailed Overview Summary
Rust provides a powerful and opinionated set of features for software partitioning, primarily revolving around **crates**, **modules**, and **workspaces**, designed to create clear boundaries and enable parallel development while ensuring memory safety and concurrency. These mechanisms are managed by Rust's build system and package manager, **Cargo**.

A **crate** is the fundamental unit of compilation, linking, versioning, and distribution in Rust. It can be either a **binary crate** (an executable program with a `main` function, typically rooted in `src/main.rs`) or a **library crate** (a collection of reusable code, typically rooted in `src/lib.rs`). Crates define explicit boundaries, manage their own dependencies specified in their `Cargo.toml` file, and expose a public API. This inherent modularity at the compilation unit level is the first step in partitioning Rust software.

Within each crate, **modules** offer a hierarchical namespace and visibility control system. Declared with the `mod` keyword, modules can be nested and can reside inline or in separate files. Items within a module (functions, structs, enums, traits, etc.) are private by default. The `pub` keyword is used to make items public, thereby defining the module's and ultimately the crate's public API. Paths (e.g., `crate::module_name::item_name`) allow unambiguous reference to items, and the `use` keyword can bring paths into scope for convenience. This system enables fine-grained control over what parts of a crate are exposed.

For larger projects, Rust offers **workspaces** to manage multiple related crates developed in tandem. A workspace is defined by a top-level `Cargo.toml` (often a "virtual manifest" without its own `[package]` section) that lists member crates. All crates in a workspace share a single `Cargo.lock` file, ensuring consistent versions for all shared dependencies, and a common build output directory (e.g., `target/`), which optimizes build times by compiling shared dependencies only once. Member crates explicitly declare dependencies on each other using path dependencies. Workspaces can also define common metadata and dependencies in `[workspace.package]` and `[workspace.dependencies]` tables for members to inherit, promoting consistency and reducing redundancy.

Effective **API design** is critical. This involves careful use of `pub` to define stable interfaces, leveraging Rust's powerful **trait system** to define shared behavior (abstract interfaces), and ensuring strong encapsulation. Robust **error handling**, primarily using the `Result<T, E>` enum and custom error types, is essential for reliable inter-component communication. Cargo orchestrates the entire lifecycle, from creation and building to testing and publishing these partitioned units.

## Key Subtopics & Initial Explanations

### 1. Modules & Visibility
*   **Detailed Explanation:** Modules in Rust, declared with `mod`, create a hierarchical structure within a crate for organizing code and controlling access. Items (functions, structs, enums, traits, other modules) are private by default. The `pub` keyword exposes items, making them part of the public API of their module and potentially the crate. Paths like `crate::foo::bar` or `super::baz` are used for addressing items. The `use` keyword brings items into the current scope for easier access but doesn't alter visibility. This system allows developers to clearly separate internal implementation details from the public contract offered by a module or crate.
*   **Key Resources for this Subtopic:**
    -   The Rust Programming Language - Ch 7, Sec 2: "Defining Modules to Control Scope and Privacy" - [https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html)
    -   Rust By Example - Modules: [https://doc.rust-lang.org/rust-by-example/mod.html](https://doc.rust-lang.org/rust-by-example/mod.html)

### 2. Crates & The Crate Root
*   **Detailed Explanation:** A crate is the smallest unit of code the Rust compiler considers. It's the unit of compilation, producing either an executable (binary crate) or a reusable library (library crate). The **crate root** is the source file from which the compiler starts building the crate; conventionally `src/main.rs` for a binary crate (which must contain `fn main()`) and `src/lib.rs` for a library crate. The content of the crate root forms the top-level module of the crate, implicitly named `crate`. The public API of a library crate is determined by the `pub` items in its crate root and any `pub` items in its public submodules.
*   **Key Resources for this Subtopic:**
    -   The Rust Programming Language - Ch 7, Sec 1: "Packages and Crates" - [https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html)
    -   Cargo Book - Project Layout: [https://doc.rust-lang.org/cargo/guide/project-layout.html](https://doc.rust-lang.org/cargo/guide/project-layout.html)

### 3. Packages & Cargo
*   **Detailed Explanation:** A Rust **package** is a collection of one or more crates (it must contain at least one, and at most one library crate) along with a `Cargo.toml` manifest file. This manifest describes the package: its name, version, authors, dependencies on other packages, build profiles, etc. **Cargo** is Rust's official build system and package manager. It uses `Cargo.toml` to compile the package's crates, fetch and compile its dependencies from `crates.io` (the Rust community crate registry) or other sources, run tests, generate documentation, and publish library crates.
*   **Key Resources for this Subtopic:**
    -   The Rust Programming Language - Ch 1, Sec 3: "Hello, Cargo!" - [https://doc.rust-lang.org/book/ch01-03-hello-cargo.html](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
    -   Cargo Book - Manifest Format: [https://doc.rust-lang.org/cargo/reference/manifest.html](https://doc.rust-lang.org/cargo/reference/manifest.html)

### 4. Workspaces for Multi-Crate Projects
*   **Detailed Explanation:** Workspaces in Cargo allow managing multiple related packages as a single unit. A top-level `Cargo.toml` file defines the workspace, listing member packages (crates). All crates in a workspace share a single `Cargo.lock` file, ensuring consistent dependency versions, and a common `target` directory for build outputs, optimizing compilation times. This is ideal for large projects where separating functionality into multiple library and binary crates improves organization and allows different parts to be developed more independently while still being managed together. The workspace `Cargo.toml` can also define shared dependency versions and package metadata that member crates can inherit using `dependency_name.workspace = true` or `package_key.workspace = true`.
*   **Key Resources for this Subtopic:**
    -   The Rust Programming Language - Ch 14, Sec 3: "Cargo Workspaces" - [https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html)
    -   Cargo Book - Workspaces: [https://doc.rust-lang.org/cargo/reference/workspaces.html](https://doc.rust-lang.org/cargo/reference/workspaces.html)

### 5. API Design with Traits and Encapsulation
*   **Detailed Explanation:** Designing clean and stable APIs is crucial for partitioned Rust software. **Encapsulation** is a core principle, with items being private by default and `pub` used to explicitly mark public interfaces. Struct fields are typically private, with access through public methods. **Traits** are central to Rust's API design for abstraction. They define a set of methods that a type must implement, acting as interfaces for shared behavior. This allows for polymorphism through trait bounds (generics) or trait objects (`dyn Trait`), enabling loose coupling as code can depend on abstract trait capabilities rather than concrete types. The **Newtype pattern** (a struct wrapping a single value) can be used to create distinct types for better type safety at API boundaries.
*   **Key Resources for this Subtopic:**
    -   The Rust Programming Language - Ch 10, Sec 2: "Traits: Defining Shared Behavior" - [https://doc.rust-lang.org/book/ch10-02-traits.html](https://doc.rust-lang.org/book/ch10-02-traits.html)
    -   Rust API Guidelines (especially sections on future-proofing like C-STRUCT-PRIVATE): [https://rust-lang.github.io/api-guidelines/future-proofing.html#c-struct-private-structs-have-private-fields-c-struct-private](https://rust-lang.github.io/api-guidelines/future-proofing.html#c-struct-private-structs-have-private-fields-c-struct-private)

### 6. Error Handling for Robust Interfaces
*   **Detailed Explanation:** Robust interfaces in Rust require clear and explicit error handling. The primary mechanism for recoverable errors is the `Result<T, E>` enum, where `T` is the type of the success value and `E` is the type of the error. Functions that can fail return a `Result`, forcing callers to handle potential errors (e.g., via `match`, `unwrap_or_else`, `?` operator). Library crates should define their own custom error types (often enums that implement `std::error::Error`) to provide meaningful error information to consumers. This makes the error contract of an API explicit. The `panic!` macro is generally reserved for unrecoverable errors indicating a bug.
*   **Key Resources for this Subtopic:**
    -   The Rust Programming Language - Ch 9: "Error Handling" - [https://doc.rust-lang.org/book/ch09-00-error-handling.html](https://doc.rust-lang.org/book/ch09-00-error-handling.html)
    -   Rust API Guidelines - Error Types (C-GOOD-ERR): [https://rust-lang.github.io/api-guidelines/interoperability.html#c-good-err-error-types-are-meaningful-and-well-behaved-c-good-err](https://rust-lang.github.io/api-guidelines/interoperability.html#c-good-err-error-types-are-meaningful-and-well-behaved-c-good-err)
