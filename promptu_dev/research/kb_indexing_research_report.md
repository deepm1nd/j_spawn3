# Knowledge Base Indexing Research Report & Proposal

## 1. Introduction
- Purpose of this report: To research and propose options for the Agentiq Knowledge Base (KB) indexing system. A core goal is to enable Agentiq AI agents, such as Jules, to effectively search this KB based on user interactions (e.g., chat messages) and for internal task processing, thereby facilitating efficient information retrieval and context-aware operations.
- This report builds upon the foundational document: `promptu_dev/meta/agentiq_indexing_scope_and_requirements.md`.

## 2. Core Requirements Summary
The Agentiq KB indexing system must support:
- **Diverse Data Types:** SME Expert Content (Markdown `*_expert.md`), SME Reference Metadata (`*_refs.md`), Master Domain Log (`master_domain_log.md`), explicit and implicit relationships, and future user annotations.
- **Key Indexing Needs:** Full-text search, semantic search (vector-based), structural awareness (Markdown sections), entity extraction, faceted search on metadata, graph traversal, and source ranking.
- **Primary Use Cases:** Keyword search, semantic search, faceted search, tag-based filtering, exact ID lookup, relationship traversal, (future) QA, and internal Agentiq component queries.
- **Performance:** Interactive queries <2s, internal queries <500ms, batch indexing in hours, incremental indexing in minutes. High precision/recall, tunable relevance. Manageable resource consumption.
- **Scalability:** From tens to thousands of documents/domains, increasing query complexity, frequent updates, and a path to distributed architecture.
- **Source Ranking:** A system to rank sources by usefulness and credibility.

## 3. Research Findings by Area

### 3.1. Text Search & Semantic Search Solutions

Two primary open-source candidates were investigated: OpenSearch and Weaviate.

-   **OpenSearch (Apache 2.0 License):**
    -   **Overview:** A mature, Elasticsearch-derived search and analytics engine with a k-NN plugin for vector search. Java-based.
    -   **Full-Text Search:** Excellent, industry-standard capabilities based on Lucene.
    -   **Vector Search:** Supports Approximate k-NN (ANN) via its k-NN plugin, offering engine choices like Faiss (HNSW, IVF, PQ/SQ quantization) and Lucene (HNSW). Also supports exact k-NN, sparse vectors, and hybrid search (combining keyword and vector scores).
    -   **Embedding Generation:** Can be done client-side or integrated via ML Commons (connectors to SageMaker, Bedrock, OpenAI, Cohere) or the `text-embedding` ingest processor.
    -   **Python Client:** `opensearch-py` (low-level, with a high-level client also available).
    -   **Pros:** Highly mature and configurable text search, rich aggregation framework for faceting, extensive operational tooling, flexible k-NN engine choices.
    -   **Cons:** Vector search is a plugin addition rather than a ground-up vector-native design. Complex graph traversals typically require application-level logic.

-   **Weaviate (BSD-3-Clause License):**
    -   **Overview:** An open-source vector database designed for semantic search, with added keyword capabilities. Go-based.
    -   **Vector Search:** Core strength. Implements HNSW for ANN, also offers `flat` and experimental `dynamic` indexes. Supports vector quantization.
    -   **Keyword Search:** Supports BM25F for keyword components in hybrid searches.
    -   **Filtering:** Emphasizes efficient pre-filtering by combining inverted indexes on scalar properties (enhanced by Roaring Bitmaps) with HNSW vector search. Advanced strategies like ACORN available.
    -   **Embedding Generation:** Strong module system for integrating various embedding providers (OpenAI, Cohere, Hugging Face, Sentence Transformers, etc.) or ingesting pre-computed vectors.
    -   **Python Client:** Official client available, supporting native and raw GraphQL queries.
    -   **Pros:** Vector-first architecture, native graph-like cross-references ideal for linked data, efficient pre-filtering for hybrid queries, excellent embedding integration.
    -   **Cons:** Text search capabilities (BM25F) might be less configurable or mature than OpenSearch/Lucene. Advanced graph analytics beyond direct link traversal might require external tools.

-   **Embedding Models:** Both OpenSearch and Weaviate are flexible, allowing the use of various models like Sentence-BERT, Universal Sentence Encoder, or OpenAI embeddings, either generated client-side or through server-side integrations. The choice of model will impact semantic quality and can be tuned independently of the database choice to some extent.

### 3.2. Structural Indexing (Markdown) & Entity Extraction (NER)

-   **Recommended Approach:** For both OpenSearch and Weaviate, client-side preprocessing is the most flexible and robust method.
-   **Markdown Parsing:**
    -   Utilize Python libraries (e.g., `markdown-it-py`, `CommonMark`) to parse Markdown into an AST or structured representation.
    -   Extract sections, sub-sections, titles, levels, and text content.
    -   Each logical chunk (e.g., section, paragraph) can then be treated as a separate "document" or sub-document for indexing and vectorization.
    -   Structural metadata (titles, levels, parent document ID) should be stored alongside the chunk.
-   **Named Entity Recognition (NER):**
    -   Employ Python libraries like `spaCy` or Hugging Face `transformers` models to extract entities (persons, organizations, locations, technologies, custom concepts).
    -   Store extracted entities as keyword arrays in the search index for filtering and faceted search.
-   **Search Engine Integration:**
    -   **OpenSearch:** Index chunks with their metadata. Nested fields or parent-child relationships (via `join` field) can model document hierarchy.
    -   **Weaviate:** Model `ExpertDocument` and `ExpertChunk` as separate classes linked by cross-references. Store entities and structural metadata as properties on `ExpertChunk`.
    -   Both platforms can effectively index and filter based on this pre-processed structured data.

### 3.3. Relationship & Graph Capabilities

-   **Explicit Relationships (e.g., domain-to-content, content-to-reference):**
    -   **Weaviate:** Natively supports this via its `cross-reference` data type. GraphQL API allows for intuitive traversal of these links (e.g., find all references mentioned in expert content within a specific domain). This is a strong fit for Agentiq's needs.
    -   **OpenSearch:** Can model with `join` fields (for strict parent-child) or by indexing arrays of IDs (e.g., `mentioned_reference_ids`). True multi-hop graph traversals usually require multiple queries and application-level joins.
-   **Implicit Relationship Discovery (e.g., content similarity, keyword co-occurrence):**
    -   This is primarily a preprocessing/analytics task external to both engines.
    -   Results (e.g., lists of related document IDs, similarity scores) can be stored as metadata in either system.
    -   Vector search itself is a powerful tool for finding implicitly related items by semantic similarity.

### 3.4. Source Ranking Mechanisms

-   **Concept:** Calculate usefulness/credibility scores for sources (especially `*_refs.md` items) and store these as metadata for sorting or boosting search results.
-   **Feature Categories for Ranking:**
    -   **Metadata-driven:** `SourceType` (e.g., journal, blog), `PublicationYear`, domain authority (if external lists available), publisher reputation.
    -   **Content-based:** Presence/quality of citations, linguistic features (clarity, objectivity via sentiment analysis), topic coherence, length/completeness.
    -   **Usage-based (Future):** User ratings, access frequency, internal KB citation counts.
    -   **Connectivity-based (Future):** PageRank-like scores if an internal citation graph is built.
-   **Ranking Models:** Start with weighted scoring/heuristics. Potentially evolve to Machine Learning (Learning to Rank) models if sufficient training data becomes available.
-   **Implementation:** Logic resides in a preprocessing pipeline. Scores are indexed as numeric fields in OpenSearch/Weaviate.

## 4. Proposed Architectural Options

The following options outline potential high-level architectures for the Agentiq KB Indexing System.

### Option 1: OpenSearch-Centric Hybrid System

-   **Description:** Utilizes OpenSearch as the primary backend for its mature text search and robust k-NN plugin for vector capabilities.
-   **Key Technologies:** OpenSearch, Python (for preprocessing: Markdown parsing, NER, embedding generation if client-side, rank feature extraction), choice of embedding models.
-   **Conceptual Data Flow:**
    1.  KB Source Files (`*.md`) ->
    2.  Python Preprocessing Pipeline:
        a.  Parse Markdown into structured chunks (title, level, content).
        b.  Extract Entities (NER).
        c.  Extract Citations (links to references).
        d.  Calculate/assign Source Rank features/scores (for references).
        e.  Generate Text Embeddings (for chunks, reference summaries).
    3.  Transformed Data (JSON) ->
    4.  OpenSearch Python Client ->
    5.  OpenSearch Cluster:
        *   `expert_chunks` index: (chunk_id, doc_id, section_title, level, text_content, vector, entities, mentioned_ref_ids).
        *   `references` index: (reference_id, url, title, authors, pub_year, source_type, tags, summary, vector, credibility_score, usefulness_score).
        *   `domains` index: (domain_id, display_name, description, key_concepts, related_domain_ids, domain_tags).
-   **Pros:**
    *   Excellent and highly configurable full-text search capabilities.
    *   Mature ecosystem, extensive documentation, and operational tooling.
    *   Flexible k-NN plugin with engine choices (Faiss for scale/quantization, Lucene for tighter filter integration on smaller sets).
    *   Strong aggregation framework for faceted search.
    *   ML Commons provides pathways for integrating embedding models server-side.
-   **Cons:**
    *   Graph-like traversals between different types of entities (e.g., domain -> content -> reference -> author) are typically application-level joins involving multiple queries.
    *   Vector search, while powerful, is a plugin; the system isn't vector-native from the ground up.
    *   Markdown structural representation and NER are fully external preprocessing steps.
-   **Addressing Key Requirements:**
    *   Semantic/Keyword/Faceted Search: Well supported.
    *   Structural Awareness/NER: Supported via preprocessing and metadata indexing.
    *   Relationships: Basic links supported by storing IDs; complex traversals are harder.
    *   Source Ranking: Scores stored and used for boosting/sorting.

### Option 2: Weaviate-Centric Vector-First System

-   **Description:** Leverages Weaviate as the primary backend, capitalizing on its vector-native design and graph-like cross-references.
-   **Key Technologies:** Weaviate, Python (for preprocessing if not using Weaviate modules for everything), choice of embedding models (via Weaviate modules or client-side).
-   **Conceptual Data Flow:**
    1.  KB Source Files (`*.md`) ->
    2.  Python Preprocessing Pipeline (can be thinner if Weaviate modules handle more):
        a.  Parse Markdown into structured chunks.
        b.  Extract Entities (NER).
        c.  Extract Citations.
        d.  Calculate/assign Source Rank features/scores.
        e.  (Optional) Generate Text Embeddings if not using a Weaviate vectorizer module.
    3.  Data Objects ->
    4.  Weaviate Python Client ->
    5.  Weaviate Instance:
        *   Schema defines classes: `ExpertDocument`, `ExpertChunk` (linked to `ExpertDocument`, contains vector, text, entities, links to `Reference`), `Reference` (contains vector, metadata, rank scores), `Domain` (links to `ExpertDocument` and other `Domain`s).
        *   Vectorizer modules (e.g., `text2vec-huggingface`) can generate embeddings on ingestion.
-   **Pros:**
    *   Vector-native architecture optimized for semantic search and similarity.
    *   Built-in graph-like capabilities with cross-references, enabling intuitive relationship traversal via GraphQL.
    *   Efficient pre-filtering for hybrid search (combining vector and scalar/keyword).
    *   Modular design for embedding generation and other functionalities.
    *   Supports HNSW, flat, and dynamic indexes, good for varied data scales.
-   **Cons:**
    *   Traditional keyword search (BM25F) might be less configurable or offer fewer advanced text analysis features than OpenSearch/Lucene.
    *   While it handles relationships well, very complex graph-specific algorithms (e.g., advanced centrality measures not based on simple traversals) might still be better suited to dedicated graph databases.
-   **Addressing Key Requirements:**
    *   Semantic/Keyword/Faceted Search: Well supported (hybrid search, strong pre-filtering).
    *   Structural Awareness/NER: Supported via preprocessing and data modeling (linked classes for structure).
    *   Relationships: Natively strong for defined links and traversals.
    *   Source Ranking: Scores stored as properties and usable in queries/ranking.

### Option 3: Polyglot Persistence - OpenSearch (Text/Vector) + Neo4j (Graph)

-   **Description:** Uses OpenSearch for its text and vector search strengths, and a dedicated graph database like Neo4j for managing and querying complex relationships.
-   **Key Technologies:** OpenSearch, Neo4j (or similar graph DB), Python (preprocessing, embedding, rank features, data loading to both DBs).
-   **Conceptual Data Flow:**
    1.  KB Source Files -> Python Preprocessing Pipeline (as in Option 1) ->
    2.  Load into OpenSearch: Text content, vectors, metadata for search and filtering.
    3.  Load into Neo4j: Extracted entities (as nodes), documents/chunks (as nodes), and relationships (e.g., `MENTIONS(ExpertChunk, Reference)`, `RELATED_TO(Domain, Domain)`, `HAS_AUTHOR(Reference, Author)`). IDs should be consistent between systems.
-   **Pros:**
    *   Best-of-breed for each function: OpenSearch for text/vector search, Neo4j for complex graph traversals and analytics.
    *   Can handle highly intricate relationship queries and graph algorithms natively in Neo4j.
-   **Cons:**
    *   Highest system complexity: Two databases to deploy, manage, secure, and keep synchronized.
    *   Data consistency and synchronization between OpenSearch and Neo4j can be challenging.
    *   Increased operational overhead and potentially cost.
    *   Application logic needs to query and combine results from two different systems.
-   **Addressing Key Requirements:**
    *   Semantic/Keyword/Faceted Search: Strong (OpenSearch).
    *   Structural Awareness/NER: Supported via preprocessing.
    *   Relationships: Very strong and native (Neo4j).
    *   Source Ranking: Scores could be computed using graph features from Neo4j and stored/used in OpenSearch.

## 5. Discussion & Recommendation Strategy

All presented options are viable. The choice depends on prioritizing specific aspects:

-   **If mature, highly configurable text search with strong operational tooling is paramount, and vector search is a critical but secondary function:** Option 1 (OpenSearch-centric) is a solid choice. Relationship querying will be less sophisticated.
-   **If native vector search performance, integrated graph-like linking for moderately complex relationships, and a streamlined embedding module system are top priorities:** Option 2 (Weaviate-centric) is very compelling. Its pre-filtering is also a strong advantage for hybrid search.
-   **If extremely complex graph analytics and real-time multi-hop relationship queries are a core, non-negotiable requirement, and the complexity of managing two systems is acceptable:** Option 3 (Polyglot with Neo4j) offers the most power in this specific dimension, at the cost of overall system complexity.

Given the Agentiq requirements, **Option 2 (Weaviate-centric)** appears to offer the best balance of strong vector search, good enough keyword search (via hybrid), native relationship handling for common use cases, and a modern, modular architecture. OpenSearch (Option 1) is a very close second, especially if its text search capabilities are deemed more critical than Weaviate's native graph links. Option 3 is likely overkill unless graph analytics become much more central than currently indicated.

**Recommendation (following user preference of 1, 3, or 4 options):**
At this stage, without deeper performance benchmarks on actual Agentiq data:
1.  **Primary Recommendation (if forced to pick one "obvious" leader for balance):** Option 2: Weaviate-Centric.
    *   *Rationale:* Its vector-native design, built-in efficient pre-filtered hybrid search, and graph-like cross-references align very well with the stated primary use cases for Agentiq KB, especially semantic understanding and connected data.
2.  **Alternative Strong Contender (presenting multiple options):** Option 1: OpenSearch-Centric.
    *   *Rationale:* If text search robustness, operational maturity, and the flexibility of its k-NN engines (especially Lucene for filtered k-NN on smaller sets) are weighted more heavily, OpenSearch is excellent.

A PoC phase would be essential to compare Option 1 and Option 2 on representative Agentiq data and queries.

## 6. Preliminary Thoughts on Next Steps

1.  **Decision on Primary Architecture:** Discuss and select one or two of the proposed architectural options for more detailed investigation or a Proof-of-Concept (PoC).
2.  **Proof-of-Concept (PoC) Development:**
    *   Select a small, representative subset of the Agentiq KB data.
    *   Implement a basic preprocessing pipeline (Markdown parsing, NER, embeddings for the chosen option).
    *   Set up the chosen search backend (OpenSearch or Weaviate).
    *   Index the sample data.
    *   Test key query types from the requirements document (keyword, semantic, faceted, relationship traversal).
    *   Evaluate ease of use, performance, and actual resource consumption at a small scale.
    *   Specifically test structural awareness and entity-based filtering.
3.  **Deeper Dive into Source Ranking:** Based on the PoC, further refine how source ranking features would be integrated and used.
4.  **Refine Preprocessing Pipeline:** Based on PoC findings, detail the components and error handling for the full preprocessing pipeline.

This report provides a foundation for making informed decisions on the technology and architecture for the Agentiq Knowledge Base indexing system.
