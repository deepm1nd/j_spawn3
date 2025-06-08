# References for Best practices for software partitioning in Java (Level 0)

Date: 2024-05-24

## Core Java Partitioning Mechanisms & API Design:

-   **[Oracle Java Tutorials - Packages]**: [https://docs.oracle.com/javase/tutorial/java/package/index.html](https://docs.oracle.com/javase/tutorial/java/package/index.html)
    *   Description: Official tutorial on creating and using packages in Java.
    *   Relevance: Primary resource for "Packages and Access Modifiers" subtopic.
-   **[Oracle Java Tutorials - Controlling Access to Members of a Class]**: [https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)
    *   Description: Explains access level modifiers (`public`, `private`, `protected`, package-private).
    *   Relevance: Supplementary resource for "Packages and Access Modifiers".
-   **[Dev.java - Introduction to Modules in Java]**: [https://dev.java/learn/modules/intro/](https://dev.java/learn/modules/intro/)
    *   Description: Modern tutorial on the Java Platform Module System (JPMS), covering `module-info.java`, `requires`, `exports`, etc.
    *   Relevance: Primary resource for "Java Platform Module System (JPMS)" subtopic.
-   **[OpenJDK - Project Jigsaw Quick Start]**: [https://openjdk.java.net/projects/jigsaw/quick-start](https://openjdk.java.net/projects/jigsaw/quick-start)
    *   Description: A quick start guide for JPMS from the OpenJDK project.
    *   Relevance: Supplementary resource for "Java Platform Module System (JPMS)".
-   **[Baeldung - Class Loaders in Java]**: [https://www.baeldung.com/java-classloaders](https://www.baeldung.com/java-classloaders)
    *   Description: Detailed article on Java ClassLoaders, their hierarchy, delegation model, and custom classloader creation.
    *   Relevance: Primary resource for "ClassLoaders and Runtime Isolation".
-   **[Oracle Java Tutorials - Finding Classes (Class Loaders - older but foundational)]**: [https://docs.oracle.com/javase/8/docs/technotes/tools/findingclasses.html](https://docs.oracle.com/javase/8/docs/technotes/tools/findingclasses.html)
    *   Description: Older Oracle documentation on how classes are found, explaining classloader concepts.
    *   Relevance: Supplementary resource for "ClassLoaders and Runtime Isolation".
-   **[Oracle Java Tutorials - Abstract Methods and Classes]**: [https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
    *   Description: Official tutorial explaining abstract classes and methods, and comparing them with interfaces.
    *   Relevance: Primary resource for "API Design: Interfaces vs. Abstract Classes".
-   **[Oracle Java Tutorials - What Is an Interface?]**: [https://docs.oracle.com/javase/tutorial/java/concepts/interface.html](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html)
    *   Description: Official tutorial defining interfaces in Java.
    *   Relevance: Supplementary resource for "API Design: Interfaces vs. Abstract Classes".
-   **[Oracle Java Tutorials - ServiceLoader]**: [https://docs.oracle.com/javase/tutorial/ext/basics/services.html](https://docs.oracle.com/javase/tutorial/ext/basics/services.html)
    *   Description: Official tutorial on using `java.util.ServiceLoader` for creating extensible applications.
    *   Relevance: Primary resource for "Service Provider Interface (SPI) Pattern".
-   **[Baeldung - Java ServiceLoader]**: [https://www.baeldung.com/java-serviceloader](https://www.baeldung.com/java-serviceloader)
    *   Description: Article explaining the `ServiceLoader` mechanism with examples.
    *   Relevance: Supplementary resource for "Service Provider Interface (SPI) Pattern".
