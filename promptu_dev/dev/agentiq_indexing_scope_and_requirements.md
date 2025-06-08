# Agentiq Knowledge Base: Indexing Scope and Requirements

## 1. Introduction

This document defines the scope, key questions, and requirements for the indexing system that will support Agentiq's knowledge base (KB). The goal is to create an efficient, scalable, and flexible indexing solution that enables powerful search, retrieval, and analysis capabilities for both user-facing queries and Agentiq's internal operations.

## 2. Data Types for Indexing

The Agentiq KB will comprise several types of interconnected data, each with specific indexing needs:

### 2.1. SME Expert Content (`*_expert.md` files)
*   **Description:** Full-text Markdown documents containing curated knowledge on specific domains. These are rich text files including headings (potentially multiple levels), paragraphs, lists, code blocks, and tables.
*   **Indexing Needs:**
    *   **Full-Text Search:** Standard keyword-based search across all content.
    *   **Semantic Search:** Ability to find conceptually similar sections or documents even if exact keywords do not match (e.g., using vector embeddings).
    *   **Structural Awareness:** Indexing of document structure (sections, sub-sections based on Markdown headings) to allow for context-aware search (e.g., "find 'X' in sections titled 'Y'").
    *   **Entity Extraction & Indexing:** Identification and indexing of named entities (persons, organizations, locations, technologies, specific concepts) within the text to enable faceted search and knowledge graph construction.
    *   **Linking to References:** Ability to identify and potentially index explicit links or mentions of reference IDs from corresponding `*_refs.md` files.
*   **Example Use Case:** User searches for "agent communication protocols." System should find sections titled "Communication in Multi-Agent Systems" and also text where specific protocols like "Contract Net" or "ACL" are discussed, even if not in section titles.

### 2.2. SME Reference Metadata (`*_refs.md` files)
*   **Description:** Files containing lists of URLs, local document paths, and associated metadata for external or internal resources.
*   **Proposed Structure (per reference):** Consider a more structured format within Markdown (e.g., YAML frontmatter per item) or a separate structured file (JSONL, CSV) per domain to explicitly capture fields like:
    *   `ReferenceID` (Unique within the file, e.g., `DOMAIN_REF_001`)
    *   `URL` or `FilePath`
    *   `Title`
    *   `Authors` (List)
    *   `PublicationYear`
    *   `SourceType` (e.g., research paper, blog post, API documentation)
    *   `UserKeywords` or `Tags` (List)
    *   `BriefSummary` or `AbstractSnippet`
*   **Indexing Needs:**
    *   Faceted search and filtering based on all structured fields (ID, Year, SourceType, Tags, Authors).
    *   Keyword search within `Title`, `UserKeywords`, `BriefSummary`.
    *   Lookup by `ReferenceID` or `URL`/`FilePath`.
*   **Example Use Case:** "Find all references tagged `#ACO` (Ant Colony Optimization) published after 2020, authored by 'M. Dorigo'."

### 2.3. Master Domain Log (`master_domain_log.md` or structured equivalent)
*   **Description:** A central registry of all known SME domains within Agentiq's KB.
*   **Proposed Structure (per domain entry):**
    *   `DomainID` (Unique identifier, e.g., `ai_agents_and_context`)
    *   `DisplayName` (Human-readable name)
    *   `Description` (Brief overview of the domain)
    *   `ExpertFilePath` (Path to the `expert.md` for this domain)
    *   `RefsFilePath` (Path to the `refs.md` for this domain)
    *   `KeyConceptsCovered` (List of strings/tags)
    *   `RelatedDomains` (List of other `DomainID`s)
    *   `DomainTags` (List of broader category tags)
*   **Indexing Needs:**
    *   Search by all fields (`DomainID`, `DisplayName`, `Description`, `KeyConceptsCovered`, `DomainTags`).
    *   Graph-based traversal of `RelatedDomains` to find interconnected knowledge areas.
*   **Example Use Case:** "Show all domains related to 'AI Safety' and also tagged `#ethics`."

### 2.4. Relationships (Implicit and Explicit)
*   **Description:**
    *   **Explicit Relationships:** Defined in `master_domain_log.md` (e.g., `RelatedDomains`).
    *   **Implicit Relationships:** Derived from content, e.g., citations/mentions of `ReferenceID`s within `*_expert.md` files; co-occurrence of `KeyConceptsCovered` or `DomainTags` across different domains; shared authors in reference files.
*   **Indexing Needs:** The indexing system should ideally support the discovery and querying of these relationships. This might involve graph database capabilities or building relationship indexes on top of more traditional search indexes.
*   **Example Use Case:** "What expert content discusses reference `ai_agents_and_context_REF_042`?" or "Which domains frequently co-mention the concepts 'Reinforcement Learning' and 'Robotics'?"

### 2.5. (Future) User Annotations and Feedback
*   **Description:** User-generated notes, ratings, or corrections on KB content.
*   **Indexing Needs:** Index annotations by user, date, associated content, keywords within the annotation. Allow filtering content based on user feedback (e.g., "show highly-rated articles on X").

## 3. Primary Use Cases for Searching & Querying

The indexing system must support various ways for users and Agentiq itself to find and utilize information:

*   **Keyword Search:** Standard free-text search across all indexed content, with relevance ranking.
*   **Semantic Search:** Find conceptually similar information even if keywords don't precisely match. Requires techniques like vector embeddings.
*   **Faceted Search:** Allow users to progressively refine search results by applying filters based on metadata fields (e.g., for references: by year, author, tag; for domains: by related domain, key concepts).
*   **Tag-Based Filtering/Search:** Retrieve all content associated with one or more specific tags.
*   **Exact ID/Field Lookup:** Quickly retrieve specific items by their unique identifiers (e.g., `ReferenceID`, `DomainID`) or specific metadata fields (e.g., URL).
*   **Relationship Traversal & Graph Queries:** Explore connections between knowledge items (e.g., "find all domains related to X," "show expert content that discusses reference Y").
*   **Question Answering (Advanced Future Goal):** Progress from extractive QA (finding relevant text passages) to abstractive QA (synthesizing novel answers based on KB content).
*   **Agentiq's Internal KB Queries:** Agentiq tasks (e.g., the research add-on, planning components) will need an efficient internal API or query language to retrieve information relevant to their operations (e.g., "retrieve all known information about topic X," "are there any guidelines related to process Y?").

## 4. Desired Performance Characteristics

*   **Search Latency:**
    *   Interactive User Queries: Target <2 seconds for common query types.
    *   Agentiq Internal Queries: Target <500ms where possible for frequent, simple lookups. More complex analytical queries may have higher latency.
*   **Indexing Latency:**
    *   Batch Indexing (Initial Setup): Acceptable time based on expected initial KB size (e.g., indexing 100 medium-sized domains within a few hours).
    *   Incremental Indexing (Delta Updates): New content added by Agentiq (e.g., via the research add-on) should be indexed and searchable within minutes.
*   **Accuracy & Relevance:** High precision (minimizing irrelevant results) and high recall (finding all relevant results) are critical. Relevance ranking should be tunable.
*   **Resource Consumption:** The indexing process and the index itself should have manageable CPU, memory, and disk space footprints relative to the size of the knowledge base.

## 5. Scalability Requirements

*   **Data Volume:** The system should be designed to scale from an initial set of tens of domains/documents to potentially hundreds or thousands in the future. Document sizes can vary.
*   **Query Complexity:** The system must handle increasingly complex queries (more filters, semantic analysis, graph traversals) as Agentiq's capabilities evolve.
*   **Update Frequency:** The KB will be dynamic, with new knowledge added regularly. The indexing system must keep pace with these updates.
*   **Architectural Scalability:** While not necessarily a day-1 requirement, the chosen indexing approach should ideally have a path towards scaling out (e.g., distributed index) if future growth demands it.

These requirements will guide the research into suitable indexing technologies and the design of Agentiq's knowledge indexing strategy.
