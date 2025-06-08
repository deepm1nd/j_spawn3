# References for Rust Software Partitioning Best Practices - L1

**Topic:** Best practices for software partitioning in Rust, focusing on modules, crates, workspaces, and API design for parallel development and clear boundaries.
**Level:** L1
**Date:** 2024-05-24

## Core Rust Language & Cargo Features

*   **The Rust Programming Language Book:**
    *   **Chapter 7: Managing Growing Projects with Packages, Crates, and Modules.** Retrieved from [https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html](https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html)
        *   Covers the fundamentals of packages, crates (as compilation units), modules for code organization, scope, privacy (`pub`), and paths (`use`). This is foundational for understanding how Rust code is structured and interfaces are defined.
    *   **Chapter 14, Section 3: Cargo Workspaces.** Retrieved from [https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html)
        *   Explains how workspaces are used to manage multiple related packages that are developed in tandem. Details shared `Cargo.lock`, output directory, and how dependencies between workspace members are handled. Essential for partitioning larger projects into multiple crates.

## API Design Guidelines

*   **Rust API Guidelines Checklist.** Retrieved from [https://rust-lang.github.io/api-guidelines/checklist.html](https://rust-lang.github.io/api-guidelines/checklist.html)
    *   Provides a comprehensive checklist for designing high-quality Rust APIs. Many points are directly relevant to creating clear boundaries and interfaces for partitioned software, including:
        *   Naming conventions (e.g., `C-CASE`, `C-ITER-TY`)
        *   Interoperability (e.g., `C-COMMON-TRAITS`, `C-CONV-TRAITS`, `C-GOOD-ERR`)
        *   Documentation (e.g., `C-CRATE-DOC`, `C-FAILURE`, `C-HIDDEN`)
        *   Predictability (e.g., `C-CTOR`)
        *   Flexibility (e.g., `C-GENERIC`, `C-OBJECT`)
        *   Type safety (e.g., `C-NEWTYPE`, `C-CUSTOM-TYPE`)
        *   Future proofing (e.g., `C-SEALED`, `C-STRUCT-PRIVATE`) - crucial for maintaining stable interfaces between modules.

## General Software Design Principles (Context)

*   While not specific to Rust, the general principles from the "Interface-Based Design Best Practices - L1" research (located in `promptu_dev/kb/interface_based_design_best_practices/expert_L1.md`) provide context:
    *   Microsoft Docs. *Best practices for RESTful web API design*. ([https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design))
    *   Wikipedia. *Modular programming*. ([https://en.wikipedia.org/wiki/Modular_programming](https://en.wikipedia.org/wiki/Modular_programming))
    *   These resources explain broader concepts of modularity, interface contracts, information hiding, and separation of concerns, which are applicable when partitioning software in any language, including Rust.
