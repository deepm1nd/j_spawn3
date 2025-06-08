# Best practices for software partitioning in Java - L0 Research Findings

Date: 2024-05-24

## L0 Detailed Overview Summary
Java offers a multi-layered approach to software partitioning, enabling developers to build modular, maintainable, and scalable applications. These mechanisms range from basic compile-time organization with **packages** and access modifiers to more robust, explicit module definitions via the **Java Platform Module System (JPMS)** introduced in Java 9. At runtime, **ClassLoaders** provide a degree of isolation and enable dynamic loading.

The foundation of Java's partitioning lies in **packages**, which group related classes and interfaces into namespaces and, in conjunction with access modifiers (`public`, `protected`, `private`, package-private), control visibility between these groups. This allows for a basic level of encapsulation.

For more robust modularity, **JPMS (Project Jigsaw)** introduced a formal module system. Modules are defined via a `module-info.java` file, specifying dependencies (`requires`), exported packages that form the module's public API (`exports`), packages opened for reflection (`opens`), and services consumed (`uses`) or provided (`provides`). This offers strong encapsulation (preventing access to non-exported packages) and reliable configuration (detecting missing modules or conflicts at launch time).

**ClassLoaders** are a core JVM mechanism responsible for loading class files. They operate hierarchically (Bootstrap, Platform/Extension, System/Application) and use a delegation model. Custom classloaders can be created to achieve more sophisticated partitioning, such as loading different versions of a library, implementing plugin systems, or providing runtime isolation for different application components (a pattern heavily used in application servers and OSGi frameworks).

Underpinning all these partitioning strategies is careful **API design**. Java's **interfaces** are the primary means of defining contracts between different software components, promoting loose coupling and allowing implementations to vary. **Abstract classes** can provide skeletal implementations or share common code among closely related classes that implement a common interface. The **Service Provider Interface (SPI)** pattern, often facilitated by `java.util.ServiceLoader`, leverages interfaces to allow for extensible applications where service implementations can be discovered and loaded at runtime.

Collectively, these features allow Java applications to be partitioned into well-defined components with clear interfaces, facilitating parallel development, enhancing reusability, and improving overall system architecture.

## Key Subtopics & Initial Explanations

### 1. Packages and Access Modifiers
*   **Detailed Explanation:** Packages in Java serve as namespaces to organize related classes and interfaces, preventing naming conflicts. They also play a crucial role in visibility control through access modifiers. `public` classes/interfaces are accessible from any other package, while those with no modifier (package-private) are only accessible within their own package. `protected` members are accessible within their own package and by subclasses in other packages, and `private` members only within their own class. This system allows developers to define public APIs for a package while hiding internal implementation details.
*   **Key Resources for this Subtopic:**
    -   Oracle Java Tutorials - Packages: [https://docs.oracle.com/javase/tutorial/java/package/index.html](https://docs.oracle.com/javase/tutorial/java/package/index.html)
    -   Oracle Java Tutorials - Controlling Access to Members of a Class: [https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)

### 2. Java Platform Module System (JPMS) - Project Jigsaw
*   **Detailed Explanation:** Introduced in Java 9, JPMS provides a higher level of aggregation and stronger encapsulation than packages alone. Modules are defined by a `module-info.java` file at the root of the module's source code. This file declares the module's name, its dependencies on other modules (`requires`), which of its own packages are exported to other modules (`exports`), which packages are opened for reflective access (`opens`), services it consumes (`uses`), and services it provides (`provides ServiceInterface with ImplementationClass;`). JPMS enforces reliable configuration by checking module dependencies and ensuring no two modules export the same package (split packages are disallowed for exported packages read by the same module). It enables the creation of custom runtime images using `jlink`.
*   **Key Resources for this Subtopic:**
    -   Dev.java - Introduction to Modules in Java: [https://dev.java/learn/modules/intro/](https://dev.java/learn/modules/intro/)
    -   Oracle - Project Jigsaw Quick Start: [https://openjdk.java.net/projects/jigsaw/quick-start](https://openjdk.java.net/projects/jigsaw/quick-start) (Often linked from official Java docs)

### 3. ClassLoaders and Runtime Isolation
*   **Detailed Explanation:** ClassLoaders are responsible for loading Java classes into the JVM at runtime. They follow a hierarchical delegation model: Bootstrap (loads core JDK classes), Platform (Java 9+, loads platform classes, previously Extension), and System/Application (loads application classes from classpath/modulepath). Each classloader defines a namespace; classes loaded by different classloaders are distinct even if they have the same name. Custom classloaders (by extending `java.lang.ClassLoader`) can be used to load classes from non-standard locations, implement versioning (loading different versions of the same class/library), or create isolated plugin/module systems where each component has its own classloader, preventing direct classpath conflicts between components.
*   **Key Resources for this Subtopic:**
    -   Baeldung - Class Loaders in Java: [https://www.baeldung.com/java-classloaders](https://www.baeldung.com/java-classloaders)
    -   Oracle Java Tutorials - Class Loaders (older but foundational): [https://docs.oracle.com/javase/8/docs/technotes/tools/findingclasses.html](https://docs.oracle.com/javase/8/docs/technotes/tools/findingclasses.html)

### 4. API Design: Interfaces vs. Abstract Classes
*   **Detailed Explanation:**
    *   **Interfaces:** Define a contract of methods that implementing classes must provide. They are the primary way to achieve API abstraction and enable polymorphism. A class can implement multiple interfaces. Since Java 8, interfaces can also have `default` methods (providing an implementation) and `static` methods. All fields in an interface are implicitly `public static final`.
    *   **Abstract Classes:** Classes declared with `abstract` cannot be instantiated. They can contain abstract methods (without implementation, which subclasses must implement), concrete methods (with implementation), and instance fields (state). A class can extend only one abstract class. Abstract classes are useful for providing a common base implementation or shared state for a group of closely related subclasses.
    *   **Usage for Partitioning:** Interfaces are generally preferred for defining public APIs between different modules/partitions due to their flexibility. Abstract classes can be used within a partition to provide skeletal implementations for a set of classes that fulfill a common interface.
*   **Key Resources for this Subtopic:**
    -   Oracle Java Tutorials - Abstract Methods and Classes: [https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
    -   Oracle Java Tutorials - What Is an Interface?: [https://docs.oracle.com/javase/tutorial/java/concepts/interface.html](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html)

### 5. Service Provider Interface (SPI) Pattern & `java.util.ServiceLoader`
*   **Detailed Explanation:** The SPI pattern is a way to achieve extensible applications and promote loose coupling between service consumers and service providers. It involves defining a service as a set of interfaces (or an abstract class). Service providers then implement these interfaces. The `java.util.ServiceLoader` class (enhanced by JPMS `uses` and `provides` directives) allows applications to discover and load implementations of a service interface at runtime. Consumers depend only on the service interface, not concrete implementations. Providers register their implementations, making them available for discovery. This is heavily used for creating plugin systems or allowing third-party extensions.
*   **Key Resources for this Subtopic:**
    -   Oracle Java Tutorials - ServiceLoader: [https://docs.oracle.com/javase/tutorial/ext/basics/services.html](https://docs.oracle.com/javase/tutorial/ext/basics/services.html)
    -   Baeldung - Java ServiceLoader: [https://www.baeldung.com/java-serviceloader](https://www.baeldung.com/java-serviceloader)
