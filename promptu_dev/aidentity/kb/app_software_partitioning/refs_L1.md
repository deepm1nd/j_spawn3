# Research Topic: Common architectures and best practices for partitioning application software.
# Research Level: L1
# Date: 2024-07-27

This L1 research builds upon the sub-topics identified in the L0 research for Application Software Partitioning. References here are typically more specific or advanced than L0 sources.

## References for L1 Research (Application Software Partitioning)

### 1. Layered Architecture (N-Tier Architecture) (L1 Depth)

*   Clean Architecture: A Craftsman's Guide to Software Structure and Design by Robert C. Martin (Uncle Bob). Prentice Hall.
    *   *Relevance:* Discusses principles like Dependency Inversion, which are key to effective layering.
*   "Service Layer Guidelines" - Martin Fowler. [https://martinfowler.com/eaaCatalog/serviceLayer.html](https://martinfowler.com/eaaCatalog/serviceLayer.html)
    *   *Relevance:* Details on implementing the Application/Service Layer.
*   "Data Transfer Object (DTO) Pattern" - Microsoft Docs or other pattern repositories.
    *   *Relevance:* Explains the use of DTOs for inter-layer communication.

### 2. Component-Based Design (CBD) (L1 Depth)

*   OSGi Alliance Specifications. [https://www.osgi.org/developer/specifications/](https://www.osgi.org/developer/specifications/)
    *   *Relevance:* Primary source for OSGi component model.
*   "Web Components specifications" - MDN Web Docs (Mozilla). [https://developer.mozilla.org/en-US/docs/Web/Web_Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components)
    *   *Relevance:* Details on modern web component technologies.
*   "Semantic Versioning (SemVer)". [https://semver.org/](https://semver.org/)
    *   *Relevance:* Strategy for managing component versioning and interface stability.

### 3. Microservice Architecture (MSA) (L1 Depth)

*   Sam Newman. (2019). *Monolith to Microservices: Evolutionary Patterns to Transform Your Monolith*. O'Reilly Media.
    *   *Relevance:* Practical guidance on migrating from monoliths to microservices.
*   "CircuitBreaker" - Martin Fowler. [https://martinfowler.com/bliki/CircuitBreaker.html](https://martinfowler.com/bliki/CircuitBreaker.html)
    *   *Relevance:* Key resiliency pattern in MSA.
*   "API Gateway Pattern" - Microservices.io by Chris Richardson. [https://microservices.io/patterns/apigateway.html](https://microservices.io/patterns/apigateway.html)
    *   *Relevance:* Details on the API Gateway pattern.
*   Consul by HashiCorp, Netflix Eureka (Open Source) - Documentation for service discovery tools.
    *   *Relevance:* Examples of service registry technologies.
*   "Saga Pattern" - Microservices.io or related articles by Caitie McCaffrey.
    *   *Relevance:* For managing distributed transactions.
*   Jaeger / Zipkin - Open source distributed tracing system documentation.
    *   *Relevance:* Tools for observability in MSA.

### 4. Event-Driven Architecture (EDA) (L1 Depth)

*   Apache Kafka Documentation. [https://kafka.apache.org/documentation/](https://kafka.apache.org/documentation/)
    *   *Relevance:* Primary source for a leading event streaming platform.
*   RabbitMQ Documentation. [https://www.rabbitmq.com/documentation.html](https://www.rabbitmq.com/documentation.html)
    *   *Relevance:* Primary source for a popular message broker.
*   "Event Sourcing Pattern" - Microsoft Azure Cloud Design Patterns. [https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)
    *   *Relevance:* Explanation of the Event Sourcing pattern.
*   "Command and Query Responsibility Segregation (CQRS) pattern" - Microsoft Azure Cloud Design Patterns. [https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs)
    *   *Relevance:* Explanation of the CQRS pattern, often used with EDA.
*   AsyncAPI Specification. [https://www.asyncapi.com/](https://www.asyncapi.com/)
    *   *Relevance:* Standard for defining asynchronous, event-driven APIs.

### 5. Domain-Driven Design (DDD) Principles for Partitioning (L1 Depth)

*   "Bounded Context" - Martin Fowler. [https://martinfowler.com/bliki/BoundedContext.html](https://martinfowler.com/bliki/BoundedContext.html)
    *   *Relevance:* Clear explanation of this core DDD concept.
*   "Strategic Domain Driven Design with Context Mapping" - InfoQ articles or presentations.
    *   *Relevance:* Deeper dives into context mapping patterns like ACL, Shared Kernel, etc.
*   Vaughn Vernon. (2016). *Domain-Driven Design Distilled*. Addison-Wesley.
    *   *Relevance:* A more concise introduction to DDD concepts.
*   "Aggregates in Domain-Driven Design" - various blog posts by DDD practitioners (e.g., Vaughn Vernon, Vlad Khononov).
    *   *Relevance:* Detailed exploration of the Aggregate pattern and its nuances.

### 6. API Design & Interface Management in Application Partitioning (L1 Depth)

*   "API-First Design" - numerous articles and blog posts (e.g., from Swagger/SmartBear, Postman).
    *   *Relevance:* Discusses the approach of designing APIs before implementation.
*   Protobuf (Protocol Buffers) Documentation. [https://developers.google.com/protocol-buffers/docs/overview](https://developers.google.com/protocol-buffers/docs/overview)
    *   *Relevance:* For gRPC and efficient data serialization.
*   GraphQL Foundation. [https://graphql.org/learn/](https://graphql.org/learn/)
    *   *Relevance:* Official learning resources for GraphQL.
*   Pact documentation (Contract Testing). [https://docs.pact.io/](https://docs.pact.io/)
    *   *Relevance:* For consumer-driven contract testing.
*   OAuth 2.0 and OpenID Connect specifications (IETF, OpenID Foundation).
    *   *Relevance:* Standards for API security (authentication and authorization).

### 7. Key Trade-offs in Application Partitioning Strategies (L1 Depth)

*   "The Strangler Fig Application" - Martin Fowler. [https://martinfowler.com/bliki/StranglerFigApplication.html](https://martinfowler.com/bliki/StranglerFigApplication.html)
    *   *Relevance:* Evolutionary approach to migrating from monoliths.
*   CAP Theorem Explained - various academic papers and blog posts (e.g., by Brewer, Gilbert & Lynch).
    *   *Relevance:* Fundamental trade-offs in distributed data systems (Consistency, Availability, Partition tolerance).
*   "Fallacies of Distributed Computing" - Papers by Peter Deutsch and others.
    *   *Relevance:* Common false assumptions made when designing distributed systems.
*   "Design for Observability" - Articles by Cindy Sridharan or Charity Majors.
    *   *Relevance:* Importance of building systems that are easy to monitor and debug, especially distributed ones.
