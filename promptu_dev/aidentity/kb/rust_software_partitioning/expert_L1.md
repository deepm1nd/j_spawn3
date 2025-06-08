# Best practices for software partitioning in Rust - L1 Research Findings

Date: 2024-05-24

---
## Research Findings for: Best practices for software partitioning in Rust, focusing on modules, crates, workspaces, and API design for parallel development and clear boundaries. (Level 1 - Aggregated Subtopics)

Date: 2024-05-24

## Content for: Modules & Visibility

*(Building upon L0 explanation with significantly more detail)*

Rust's module system is a cornerstone of its ability to create well-defined, encapsulated components, which is essential for effective software partitioning. While L0 covered the basics of `mod` and `pub`, an L1 understanding requires delving into finer-grained visibility control, path resolution nuances, and common structuring patterns.

**Detailed Visibility Control:**
-   **`pub`:** As established, `pub` makes an item public to its parent module's scope. For an item to be part of a crate's external public API, it and all its parent modules up to the crate root must be `pub`.
-   **Struct Field Visibility:** When a struct is `pub`, its fields are still private by default unless individually marked `pub`. This enforces encapsulation even for public data structures.
    ```rust
    pub mod my_module {
        pub struct MyStruct {
            pub public_field: i32,
            private_field: String, // Not accessible directly outside my_module
        }
        impl MyStruct {
            pub fn new(val: i32, secret: String) -> Self {
                Self { public_field: val, private_field: secret }
            }
            // Method to access private_field if needed
            pub fn get_secret_len(&self) -> usize {
                self.private_field.len()
            }
        }
    }
    ```
-   **`pub(crate)`:** This makes an item visible anywhere within the current crate but not outside. It's useful for exposing internal helper functions or types that should be shared across different modules of the same crate but not become part of its external API.
-   **`pub(in path)`:** Allows specifying a more precise scope for visibility. For example, `pub(in crate::some_module)` makes an item visible throughout `some_module` and its descendants, as if it were defined in `some_module`'s parent.
-   **`pub(super)`:** Makes an item visible to the immediate parent module (`super`).

**Path Resolution and `use`:**
-   **Paths:** Items are referred to by their path, starting from `crate` (the current crate's root), `super` (parent module), or `self` (current module).
-   **The `use` Statement:** Brings paths into the current scope for shorthand. It does *not* affect visibility but significantly improves readability. `pub use` is a powerful tool for re-exporting items, allowing a crate to present a public API that might be structured differently from its internal module organization. This decouples the public API from internal refactoring.
    ```rust
    // In lib.rs or a public module
    mod internal_details {
        pub fn useful_function() { /* ... */ }
    }
    // Re-export useful_function as part of the crate's top-level API
    pub use internal_details::useful_function;
    ```
    Consumers can then call `my_crate::useful_function()` without needing to know about `internal_details`.

**File System Mapping:**
-   If `mod foo;` is declared in `src/lib.rs` (or any `bar.rs`), the compiler looks for:
    1.  `src/foo.rs` (or `bar/foo.rs`)
    2.  `src/foo/mod.rs` (or `bar/foo/mod.rs`)
-   This allows for flexible organization of module contents into separate files or directories.

**Common Patterns:**
-   Often, a library will have a relatively lean `src/lib.rs` that primarily uses `pub mod` and `pub use` statements to define and export its public API from functionality implemented in various submodules.
-   Internal helper modules might be kept private or `pub(crate)`.

**Pros & Cons:**
-   **Pros:** Strong encapsulation by default, fine-grained control over API surfaces, clear separation of concerns, helps prevent unintended coupling.
-   **Cons/Considerations:** Can lead to complex pathing if not organized well; initial learning curve for visibility and path rules. Overuse of `pub` can diminish encapsulation benefits.

**Resources:** (Includes L0 resources, assumes new searches might find more specific blog posts or advanced guides for L1)
-   The Rust Programming Language - Ch 7: [https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html](https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html)
-   Rust Reference - Visibility and Privacy: [https://doc.rust-lang.org/reference/visibility-and-privacy.html](https://doc.rust-lang.org/reference/visibility-and-privacy.html)
-   Rust by Example - Modules, Visibility, Struct Visibility, `use` declaration, `super` and `self`, File hierarchy: ([https://doc.rust-lang.org/rust-by-example/mod.html](https://doc.rust-lang.org/rust-by-example/mod.html))

## Content for: Crates & The Crate Root

*(Building upon L0 explanation with significantly more detail)*

In Rust, a **crate** is the fundamental unit of software compilation and distribution. It serves as a container for Rust code, defining a distinct scope and enabling modular design. Each crate is compiled independently, producing either an executable binary or a reusable library.

**Types of Crates:**
1.  **Binary Crate:**
    *   Purpose: To create an executable program that can be run directly.
    *   Entry Point: Must contain a `fn main()` function, which is where program execution begins.
    *   Crate Root: Conventionally, the source file `src/main.rs` is the root of a binary crate. The package name (defined in `Cargo.toml`) usually matches the binary name.
    *   Output: An executable file.
2.  **Library Crate:**
    *   Purpose: To provide reusable functionality that can be shared across multiple projects. Library crates are the cornerstone of Rust's ecosystem on `crates.io`.
    *   Entry Point: Does not have a `main` function.
    *   Crate Root: Conventionally, the source file `src/lib.rs` is the root of a library crate.
    *   Output: A compiled library file (e.g., `.rlib` or `.so`/`.dll`/`.dylib` depending on crate type and compilation targets).
    *   Public API: The items (functions, structs, enums, traits, modules) marked `pub` in `src/lib.rs` and its public submodules constitute the library's public interface that other crates can use.

**Key Characteristics for Partitioning:**
*   **Compilation Unit:** The compiler processes each crate as a whole. This means type checking, borrow checking, and code generation are performed on a per-crate basis.
*   **Namespace Boundary:** Each crate has its own top-level namespace. Items from other crates must be explicitly brought into scope via `extern crate` (older Rust editions) or by simply adding them to `Cargo.toml` and using `use crate_name::item;` (Rust 2018+).
*   **Dependency Scope:** Dependencies are managed per crate in its `Cargo.toml`. This allows different crates, even within the same workspace, to potentially have different (though often coordinated) sets of dependencies.
*   **Versioning and Distribution:** Library crates are typically versioned using Semantic Versioning (SemVer) and can be published to `crates.io`, making them discoverable and usable by the wider Rust community. This versioning is critical for managing evolving APIs between dependent crates.

**The Crate Root:**
The **crate root** (`src/lib.rs` or `src/main.rs`) is the starting point for the compiler. The code in this file forms the root module of the crate's module hierarchy, implicitly named `crate`. All paths to items within the crate start from this root (e.g., `crate::my_module::my_item`). The structure of the crate root, particularly its public exports (`pub mod`, `pub use`), defines the top-level API that the crate presents to its users or to other parts of a larger application.

Understanding the distinction between binary and library crates, and the role of the crate root, is essential for structuring any non-trivial Rust project effectively for partitioning and reuse.

**Resources:**
-   The Rust Programming Language - Ch 7, Sec 1: "Packages and Crates" - [https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html)
-   Cargo Book - Project Layout: [https://doc.rust-lang.org/cargo/guide/project-layout.html](https://doc.rust-lang.org/cargo/guide/project-layout.html)
-   Rust Reference - Crates and Source Files: [https://doc.rust-lang.org/reference/crates-and-source-files.html](https://doc.rust-lang.org/reference/crates-and-source-files.html)

## Content for: Workspaces for Multi-Crate Projects

*(Building upon L0 explanation with significantly more detail)*

Cargo **workspaces** are a key feature in Rust for managing multiple related packages (crates) that are developed and built together. This is particularly useful for partitioning large applications or libraries into smaller, more focused components, while maintaining a unified build process and dependency management.

**Core Concepts & Structure:**
A workspace is defined by a top-level `Cargo.toml` file that includes a `[workspace]` table. This root manifest typically acts as a "virtual manifest," meaning it doesn't define a `[package]` section for itself but rather orchestrates the member packages.
-   **`[workspace]` table Attributes:**
    -   `members`: An array of strings specifying the paths to the individual crate directories that form the workspace (e.g., `members = ["app_cli", "core_lib", "utils/parser_lib"]`). Glob patterns like `crates/*` are also supported.
    -   `resolver` (optional but recommended): Specifies the dependency resolver version (e.g., `"2"`) to ensure consistent and improved dependency resolution behavior across all member crates. Introduced in Rust 1.51.
    -   `exclude`: An array of strings to explicitly exclude certain packages from the workspace, even if they fall within a `members` glob pattern or are path dependencies.
    -   `default-members`: An optional array specifying which member(s) should be built, run, or tested if Cargo commands are executed in the workspace root without specific package selection flags (`-p` or `--workspace`). If not set, and the root is a virtual manifest, all members are typically processed.
-   **Shared `Cargo.lock` File:** All crates within the workspace share a single `Cargo.lock` file located in the workspace root. This is a crucial feature, as it ensures that all member crates and their transitive dependencies use the exact same resolved versions for all shared dependencies. This prevents integration issues due to version mismatches and ensures build reproducibility across the entire workspace.
-   **Shared `target` Directory:** All compiled artifacts (binaries, libraries, documentation, test executables) from all member crates are placed in a common `target/` directory within the workspace root. This prevents redundant compilation of shared dependencies, speeding up overall build times for the workspace.

**Defining Member Crates and Dependencies:**
-   Each directory listed in the `members` array of the workspace `Cargo.toml` must contain its own standard `Cargo.toml` file, defining it as a regular Rust package (either a library or a binary crate).
-   Dependencies between member crates within the workspace are specified using **path dependencies** in their respective `Cargo.toml` files. For example, if `app_cli` depends on `core_lib` (both members of the same workspace):
    ```toml
    # In app_cli/Cargo.toml
    [dependencies]
    core_lib = { path = "../core_lib" }
    ```
-   External dependencies (from `crates.io` or git repositories) are specified in each member crate's `Cargo.toml` as usual.

**Inheriting Dependencies and Package Metadata (Rust 1.64+):**
Workspaces offer powerful features to reduce redundancy and maintain consistency across member crates:
-   **`[workspace.dependencies]`**: Common dependencies and their versions can be defined in the workspace root `Cargo.toml`. Member crates can then inherit these by specifying `dependency_name.workspace = true` in their own `[dependencies]`, `[dev-dependencies]`, or `[build-dependencies]` sections, optionally adding or enabling specific features.
    ```toml
    # Workspace root Cargo.toml
    [workspace.dependencies]
    serde = { version = "1.0.150", features = ["derive"] }
    tokio = { version = "1.20", features = ["macros", "rt-multi-thread"] }

    # Member crate's Cargo.toml
    [dependencies]
    serde.workspace = true # Inherits version "1.0.150" and "derive" feature
    tokio = { workspace = true, features = ["time"] } # Inherits version "1.20", adds "time" feature (final features are additive)
    ```
-   **`[workspace.package]`**: Common package metadata keys (e.g., `authors`, `version`, `edition`, `license`, `repository`, `rust-version`) can be defined once in the workspace root. Member crates inherit these values using `{key}.workspace = true` (e.g., `version.workspace = true`). This ensures consistency for all published crates from the workspace.

**Benefits for Partitioning and Parallel Development:**
-   **Stronger Modularity & Clear Boundaries:** Each crate is a distinct compilation unit with its own API.
-   **Organized Codebase:** Large systems are broken into smaller, more focused components.
-   **Independent Development:** Teams can work on different crates more independently, respecting defined APIs. The shared lock file ensures integration uses consistent dependency versions.
-   **Reduced Build Times & Consistent Dependencies:** Centralized lock file and target directory.
-   **Simplified Management:** Common dependencies and metadata can be managed in one place.
-   **Facilitates Monorepo Strategy:** Enables keeping multiple related services or libraries in a single repository but partitioned into distinct, buildable crates.

**Resources:**
-   The Rust Programming Language - Ch 14.3: "Cargo Workspaces" - [https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html)
-   Cargo Book - Workspaces: [https://doc.rust-lang.org/cargo/reference/workspaces.html](https://doc.rust-lang.org/cargo/reference/workspaces.html)
-   Effective Rust - Workspaces: [https://www.lurklurk.org/effective-rust/workspaces.html](https://www.lurklurk.org/effective-rust/workspaces.html)

## Content for: API Design with Traits and Encapsulation

*(Building upon L0 explanation with significantly more detail)*

Rust's API design philosophy centers on safety, explicitness, and enabling robust abstractions, which are crucial for effective software partitioning. Key elements include strong encapsulation enforced by the module system and the powerful trait system for defining interfaces.

**Encapsulation: The Foundation of Clear Boundaries**
-   **Privacy by Default:** All items in Rust (functions, structs, enums, modules, etc.) are private by default. This is a fundamental design choice that forces developers to be intentional about what constitutes a module's or crate's public API.
-   **The `pub` Keyword:** Explicitly marks items as public. For an item to be part of a crate's external API, it and all its parent modules must be `pub`.
-   **`pub(crate)`:** Restricts visibility to the current crate, useful for internal helper functions or types shared across modules within the same crate but not exposed externally.
-   **`pub(in path)` and `pub(super)`:** Allow for even finer-grained visibility control within a crate's module hierarchy.
-   **Struct Field Privacy (`C-STRUCT-PRIVATE` from API Guidelines):** Even if a struct is `pub`, its fields remain private unless explicitly marked `pub`. This is critical for maintaining invariants and allowing internal refactoring of a struct without breaking external code. Public access to struct data should generally be through `pub` methods (getters, setters, or methods that operate on the data).
    ```rust
    pub struct DataProcessor {
        pub processed_items_count: usize,
        internal_cache: std::collections::HashMap<String, String>, // Private
        api_key: String, // Private
    }
    impl DataProcessor {
        pub fn new(key: String) -> Self { Self { processed_items_count: 0, internal_cache: Default::default(), api_key: key } }
        pub fn process(&mut self, data: &str) -> Result<(), String> { self.processed_items_count +=1; /* ... */ Ok(())}
    }
    ```
-   **Newtype Pattern (`C-NEWTYPE`, `C-NEWTYPE-HIDE`):** Wrapping a primitive or existing type in a new struct (e.g., `pub struct CustomerId(u64);`) creates a distinct type at compile time. This prevents accidental misuse (e.g., passing a `ProductId` where a `CustomerId` is expected). The inner type can be kept private if it's an implementation detail, with the newtype exposing specific methods, thus enhancing both type safety and encapsulation at API boundaries.

**Traits as Abstract Interfaces (`C-OBJECT`):**
Traits are Rust's primary mechanism for defining shared behavior and abstract interfaces.
-   **Defining Behavior:** A trait defines a set of method signatures that a type must implement to conform to that trait. Default implementations can also be provided.
    ```rust
    pub trait DataExporter {
        fn export_data(&self, data: &serde_json::Value) -> Result<String, ExportError>;
        fn format_name(&self) -> &'static str; // e.g., "JSONExporter", "CSVExporter"
        fn requires_authentication(&self) -> bool { false } // Default implementation
    }
    #[derive(Debug)] // Example custom error type
    pub struct ExportError(String);
    ```
-   **Polymorphism:**
    -   **Trait Bounds (Static Dispatch):** Functions and structs can use generics with trait bounds to operate on any type that implements a specific trait (e.g., `fn send_report<E: DataExporter>(exporter: &E, report_data: &serde_json::Value)`). This is resolved at compile time (monomorphization) and is highly performant.
    -   **Trait Objects (Dynamic Dispatch):** Using `Box<dyn DataExporter>` or `&dyn DataExporter` allows for storing different concrete types that implement the `DataExporter` trait in a collection or passing them around when the exact type isn't known until runtime. This requires the trait to be "object-safe" (e.g., methods must not return `Self`, not take `Self` by value, or have generic type parameters not related to `Self`).
-   **Decoupling Crates:** Traits are fundamental for decoupling crates. One crate can define and export a trait (the interface), and other crates can depend on this trait and provide implementations, or consume types that implement the trait, without needing to know about each other's concrete types.

**API Stability and Future Proofing (`C-SEALED`):**
-   **Minimize Public Surface:** The smaller and more deliberate your public API, the easier it is to maintain and evolve without breaking changes.
-   **Sealed Traits:** If you define a trait that you intend for users of your crate to use but *not* to implement externally (to reserve the ability to add new methods to the trait later without it being a breaking change for downstream implementers), you can use the "sealed trait" pattern. This typically involves a private supertrait that only types within your crate can implement.

**Resources:**
-   The Rust Programming Language - Ch 10, Sec 2: "Traits: Defining Shared Behavior" - [https://doc.rust-lang.org/book/ch10-02-traits.html](https://doc.rust-lang.org/book/ch10-02-traits.html)
-   Rust API Guidelines (various sections): [https://rust-lang.github.io/api-guidelines/checklist.html](https://rust-lang.github.io/api-guidelines/checklist.html)
-   Rust Design Patterns - Newtype: [https://rust-unofficial.github.io/patterns/patterns/behavioural/newtype.html](https://rust-unofficial.github.io/patterns/patterns/behavioural/newtype.html)
-   Rust Book - Ch 19, Sec 3: "Advanced Traits" (for more on associated types, supertraits, newtype for trait implementation rules) - [https://doc.rust-lang.org/book/ch19-03-advanced-traits.html](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html)

---
*(Content for "Packages & Cargo", "Error Handling for Robust Interfaces", and "Benefits for Parallel Development" would be similarly expanded here.)*
