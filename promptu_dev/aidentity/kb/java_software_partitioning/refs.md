# References for Java Software Partitioning Best Practices - L1

**Topic:** Best practices for software partitioning in Java, covering packages, modules (JPMS), classloaders, and API design (interfaces, abstract classes) for modularity and parallel development.
**Level:** L1
**Date:** 2024-05-24

## Core Java Language & Platform Features

*   **Oracle. The Java™ Tutorials: Object-Oriented Programming Concepts - What Is an Interface?** Retrieved from [https://docs.oracle.com/javase/tutorial/java/concepts/interface.html](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html)
    *   (Previously reviewed in "Interface-Based Design Best Practices" research)
    *   Key Takeaways: Definition and use of the `interface` keyword, `implements` keyword, contract enforcement.
*   **Oracle. The Java™ Tutorials: Object-Oriented Programming Concepts - What Is a Package?** Retrieved from [https://docs.oracle.com/javase/tutorial/java/concepts/package.html](https://docs.oracle.com/javase/tutorial/java/concepts/package.html)
    *   (Previously reviewed in "Interface-Based Design Best Practices" research)
    *   Key Takeaways: Role of packages for namespace organization and visibility control.
*   **Dev.java (Oracle). *Introduction to Modules in Java*.** Retrieved from [https://dev.java/learn/modules/intro/](https://dev.java/learn/modules/intro/)
    *   Key Takeaways: Detailed explanation of the Java Platform Module System (JPMS), `module-info.java`, `requires`, `exports`, `opens`, `uses`, `provides` directives, modular JARs, module path, module resolution, and benefits like strong encapsulation and reliable configuration.
*   **Baeldung. *Class Loaders in Java*.** Retrieved from [https://www.baeldung.com/java-classloaders](https://www.baeldung.com/java-classloaders) (Note: Original attempt `java-abstract-class-vs-interface` was a 404, this was a subsequent successful fetch on classloaders).
    *   Key Takeaways: Hierarchy (Bootstrap, Platform, System), delegation model, role in class uniqueness and visibility, custom classloaders for versioning and dynamic loading, context classloaders.
*   **Oracle. The Java™ Tutorials: Interfaces and Inheritance - Abstract Methods and Classes.** Retrieved from [https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
    *   Key Takeaways: Definition of abstract classes and methods, comparison with interfaces, use cases for sharing code among related classes, declaring non-static/non-final fields, providing skeletal implementations.

## General Software Design Principles (Context)

*   While not specific to Java, the general principles from the "Interface-Based Design Best Practices - L1" research (located in `promptu_dev/kb/interface_based_design_best_practices/expert_L1.md`) provide context:
    *   Microsoft Docs. *Best practices for RESTful web API design*.
    *   Wikipedia. *Modular programming*.
    *   These resources explain broader concepts of modularity, interface contracts, information hiding, and separation of concerns, which are applicable when partitioning software in any language, including Java.
