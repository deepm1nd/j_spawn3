# References for Best practices for software partitioning in Java (Level 1)

Date: 2024-05-24

## Core Java Partitioning Mechanisms & API Design (Sources used for L0 and deepened for L1):

-   **[Oracle Java Tutorials - Packages]**: [https://docs.oracle.com/javase/tutorial/java/package/index.html](https://docs.oracle.com/javase/tutorial/java/package/index.html)
    *   Description: Official tutorial on creating and using packages in Java.
    *   Relevance: Foundational for L0; re-consulted for L1 details on "Packages and Access Modifiers," focusing on namespace management and compile-time visibility.
-   **[Oracle Java Tutorials - Controlling Access to Members of a Class]**: [https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)
    *   Description: Explains access level modifiers (`public`, `private`, `protected`, package-private) and their impact on API definition.
    *   Relevance: Foundational for L0; re-consulted for L1 details on "Packages and Access Modifiers."
-   **[Dev.java - Introduction to Modules in Java]**: [https://dev.java/learn/modules/intro/](https://dev.java/learn/modules/intro/)
    *   Description: Modern tutorial on the Java Platform Module System (JPMS), covering `module-info.java`, `requires` (including `transitive` and `static`), `exports` (including qualified), `opens` (including qualified), `uses`, `provides`, automatic modules, and the unnamed module.
    *   Relevance: Primary resource for L0 and L1 deep dive on "Java Platform Module System (JPMS)".
-   **[OpenJDK - Project Jigsaw Quick Start]**: [https://openjdk.java.net/projects/jigsaw/quick-start](https://openjdk.java.net/projects/jigsaw/quick-start)
    *   Description: A quick start guide for JPMS from the OpenJDK project, useful for understanding basic module creation and commands.
    *   Relevance: Supplementary L0 and L1 resource for "Java Platform Module System (JPMS)".
-   **[Baeldung - Class Loaders in Java]**: [https://www.baeldung.com/java-classloaders](https://www.baeldung.com/java-classloaders)
    *   Description: Detailed article on Java ClassLoaders, their hierarchy (Bootstrap, Platform, System), delegation model, and creation/use of custom classloaders for runtime isolation and versioning.
    *   Relevance: Primary resource for L0 and L1 research on "ClassLoaders and Runtime Isolation".
-   **[Oracle Java Tutorials - Abstract Methods and Classes]**: [https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
    *   Description: Official tutorial explaining abstract classes and methods, comparing them with interfaces, and detailing use cases for shared code and state.
    *   Relevance: Primary L0 and L1 resource for "API Design: Interfaces vs. Abstract Classes".
-   **[Oracle Java Tutorials - What Is an Interface?]**: [https://docs.oracle.com/javase/tutorial/java/concepts/interface.html](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html)
    *   Description: Official tutorial defining interfaces, including default and static methods.
    *   Relevance: Supplementary L0/L1 resource for "API Design: Interfaces vs. Abstract Classes".
-   **[Oracle Java Tutorials - ServiceLoader]**: [https://docs.oracle.com/javase/tutorial/ext/basics/services.html](https://docs.oracle.com/javase/tutorial/ext/basics/services.html)
    *   Description: Official tutorial on using `java.util.ServiceLoader` for creating extensible applications, a key pattern for SPI.
    *   Relevance: Primary L0 and L1 resource for "Service Provider Interface (SPI) Pattern".
-   **[Baeldung - Java ServiceLoader]**: [https://www.baeldung.com/java-serviceloader](https://www.baeldung.com/java-serviceloader)
    *   Description: Article explaining the `ServiceLoader` mechanism with practical examples.
    *   Relevance: Supplementary L0/L1 resource for "Service Provider Interface (SPI) Pattern".

## Illustrative Additional L1+ Resources (Representing the type of sources sought for deeper L1 understanding):

-   **Effective Java by Joshua Bloch (Book - relevant items like Item 15, Item 18, etc.)**
    *   Description: Provides in-depth best practices for Java programming, including API design, minimizing accessibility, favoring composition over inheritance, and effective use of interfaces and abstract classes.
    *   Relevance: L1+ depth for API design, encapsulation, and general best practices for creating robust components.
-   **"The Java Module System" by Nicolai Parlog (Book)**
    *   Description: A comprehensive guide dedicated to JPMS, covering advanced features, migration strategies, patterns, and pitfalls.
    *   Relevance: L1+ in-depth understanding of JPMS beyond introductory tutorials.
-   **Oracle Documentation - `java.lang.Module` API**: [https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Module.html](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Module.html)
    *   Description: API documentation for the `Module` class, providing insights into how modules are represented and can be introspected at runtime.
    *   Relevance: L1+ details on runtime aspects of JPMS.
