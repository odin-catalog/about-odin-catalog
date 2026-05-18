# ODIN Catalog - Open Data Intelligence for the Modern Enterprise

> **Alpha** - APIs and schemas may change between releases.

ODIN is an open-source data catalog built on W3C standards. Business semantics, end-to-end lineage, and AI-powered discovery - designed for enterprises where data governance isn't optional.

**Standards:** DCAT 3.0 · DPROD · OpenLineage · FIBO · CSV-W · Apache AGE · Spring AI

---

## The Problem

Data catalogs stopped solving problems a decade ago. Most are glorified spreadsheets with a search bar. They document data, but don't understand it.

| | |
|---|---|
| **70%** | of a data analyst's time is spent finding and understanding data - not using it. |
| **3×** | more incidents caused by unknown data lineage than any other governance failure *(Gartner, 2024)*. |
| **$15M** | average cost of a BCBS 239 compliance failure for a Tier 1 bank *(Basel Committee, 2023)*. |

### Discovery without understanding
Existing catalogs find the table. They don't tell you what `TRADE_AMT` actually means, whether it's gross or net, or what currency it's in.

### Lineage is an afterthought
Lineage is either absent, stale, or requires manual curation. When a pipeline breaks, nobody knows what downstream reports are affected.

### Governance is a checkbox
Regulatory requirements like BCBS 239, FRTB, and GDPR demand documented lineage and semantic consistency. Most orgs scramble at audit time.

---

## Capabilities

ODIN treats metadata as a first-class concern - standards-based, semantically rich, and lineage-aware from day one.

### Semantic Vocabulary Mappings
Map every data element to a controlled vocabulary concept using SKOS matching properties. Ships with schema.org, FIBO FND/FBC/SEC/MD, SKOS, and Dublin Core pre-loaded. Add your own domain ontology in minutes.

`FIBO` `schema.org` `SKOS` `Custom Vocabularies`

### End-to-End Data Lineage
Ingest lineage from any OpenLineage-compatible tool (Spark, dbt, Airflow). Parse SQL DDL across Snowflake, Teradata, and Hive dialects. Traverse multi-hop graphs with Apache AGE - upstream, downstream, and column-level.

`OpenLineage` `Apache AGE` `DDL Parsing` `Column-Level`

### Data Product Governance
Model data products with the DPROD standard. Track lifecycle stages from Ideation through Consume. Define input/output ports, link distributions, and attach access policies - all through a structured API.

`DPROD` `Lifecycle` `Ports` `Access Policy`

### AI-Powered Discovery
Ask questions in plain English. ODIN uses retrieval-augmented generation over your full metadata corpus - descriptions, logical elements, and vocabulary mappings. Runs locally on Ollama or via OpenAI. No data leaves your perimeter.

`RAG` `Ollama` `pgvector` `SSE Streaming`

### Automated Metadata Harvest
Connect to Snowflake, AWS Glue, Teradata, or any DCAT HTTP endpoint. Scheduled Spring Batch jobs crawl schemas, infer types, publish events to Kafka, and auto-generate draft logical models from physical columns.

`Snowflake` `AWS Glue` `Teradata` `DCAT HTTP`

### Faceted Semantic Search
Full-text and semantic search over OpenSearch. Filter by entity type, lifecycle, vocabulary concepts, FIBO ontology terms, or whether a dataset has lineage and a published logical model. Autocomplete suggestions in milliseconds.

`OpenSearch` `FIBO Facets` `Autocomplete` `Semantic`

---

## Built on Open Standards

ODIN's metamodel is grounded in W3C, OMG, and FIBO standards. Your metadata is portable by design. No proprietary lock-in.

| Standard | Body | Purpose |
|---|---|---|
| **DCAT 3.0** | W3C | Data Catalog Vocabulary. Interoperable catalog and dataset descriptions in JSON-LD. Export your full catalog as machine-readable linked data. |
| **DPROD** | OMG | Data Product standard from the Object Management Group. Defines ownership, ports, lifecycle, and access contracts for data products. |
| **CSV-W** | W3C | CSV on the Web. Formally describes the physical schema of tabular data - column names, types, constraints - linked to logical elements. |
| **OpenLineage** | Linux Foundation | Open standard for data lineage collection. Compatible with Spark, dbt, Airflow, Flink, and 30+ integrations out of the box. |
| **FIBO** | EDM Council | Financial Industry Business Ontology. Industry-standard vocabulary for financial instruments, parties, contracts, and processes. FND · FBC · SEC · MD · BP |
| **SKOS** | W3C | Simple Knowledge Organization System. `exactMatch`, `closeMatch`, `relatedMatch` - precise semantic relationships between data elements and concepts. |

---

## Data Lineage

Trace every byte from source to report. ODIN builds a live graph of your data pipelines - automatically. SQL DDL parsing extracts lineage from `CREATE VIEW` statements without touching your pipelines.

- **Multi-hop traversal** - Query upstream or downstream lineage up to N hops via Apache AGE Cypher queries.
- **Column-level lineage** - Track individual column transformations across jobs - essential for BCBS 239 and GDPR data element tracing.
- **Impact analysis** - Before changing a source table, know every downstream report, model, and data product that will be affected.

### Example: Upstream lineage · `REGULATORY_DB.BCBS239` · depth 4

```
REGULATORY_DB.BCBS239.RISK_AGGREGATION
  └─ 3 inputs
      ├─ RISK_DB.MARKET_RISK.DAILY_POSITIONS
      │    ├─ TRADING_DB.BLOTTER.TRADE_BLOTTER
      │    └─ kafka://prices-realtime
      │
      ├─ RISK_DB.PNL.DAILY_ATTRIBUTION
      │    ├─ TRADING_DB.BLOTTER.TRADE_BLOTTER
      │    └─ REFDATA_DB.EQUITIES.SECURITIES_MASTER
      │
      └─ RISK_DB.CREDIT.COUNTERPARTY_EXPOSURE
           ├─ TRADING_DB.BLOTTER.TRADE_BLOTTER
           └─ REFDATA_DB.COUNTERPARTY.MASTER

10 unique nodes · 8 DERIVED_FROM edges · via Apache AGE Cypher
```

---

## Use Case: Financial Services

ODIN ships with the Financial Industry Business Ontology (FIBO) pre-loaded. Map your trade and risk data to an industry-standard semantic layer in minutes.

**01 · BCBS 239 Risk Data Aggregation**
Documented lineage from source systems to regulatory reports. Automated tracing of every data element that flows into capital calculations.

**02 · FRTB Capital Calculation Audit Trail**
Column-level lineage from market data feeds through risk positions to SA capital charges. Auditable and reproducible.

**03 · Semantic Reference Data Management**
Map ISIN, LEI, CUSIP, and monetary amounts to FIBO concepts. Eliminate the ambiguity between source systems with a canonical semantic layer.

**04 · Data Product Marketplaces**
Publish governed data products with defined SLAs, access policies, and lineage. Consumers find what they need; owners control who gets it.

### FIBO Vocabulary Mappings - Trade Blotter · Logical Model v2.0

| Business Name | Match Type | FIBO Concept |
|---|---|---|
| Trade Amount | `exactMatch` | `FND/…/MonetaryAmount` |
| Settlement Currency | `exactMatch` | `FND/…/Currency` |
| Counterparty LEI | `exactMatch` | `FBC/…/LegalEntityIdentifier` |
| Instrument ISIN | `exactMatch` | `FBC/…/ISIN` |
| Market Price | `exactMatch` | `MD/…/MarketPrice` |
| Financial Instrument | `exactMatch` | `FBC/…/FinancialInstrument` |
| Trade Date | `closeMatch` | `schema:startDate` |

*7 of 10 elements mapped · 0 unbound · SKOS match types*

---

## Architecture

Open source. API-first. Open-standards.

Six Spring Boot microservices, each owning its data store. Deploy to Kubernetes, Docker Compose, or your cloud of choice. No managed service required.

### Services

| Service | Port | Responsibility |
|---|---|---|
| **catalog-service** | 8001 | DCAT/DPROD/CSV-W metadata. Logical models. Vocabulary mappings. Kafka event publisher. |
| **harvest-service** | 8002 | Spring Batch crawlers for Snowflake, Glue, Teradata, DCAT HTTP. Quartz scheduler. |
| **lineage-service** | 8003 | OpenLineage ingestion. DDL parsing via Apache Calcite. Apache AGE Cypher graph queries. |
| **search-service** | 8004 | OpenSearch indexing with FIBO facets. Autocomplete. Full-text + semantic hybrid search. |
| **ai-service** | 8005 | Spring AI RAG pipeline. pgvector embeddings. Ollama (local) or OpenAI. SSE chat streaming. |
| **identity-service** | 8006 | Keycloak OAuth2/OIDC integration. ABAC policies. API keys. Multi-tenant isolation. |

### Data Stores

| Store | Role |
|---|---|
| **PostgreSQL 16** | Metadata, harvest, identity, and AI conversation stores. |
| **Apache AGE** | Lineage graph on PostgreSQL. Cypher queries. No separate graph DB. |
| **pgvector** | Embedding vectors for RAG. IVFFlat index. 768 dimensions. |
| **OpenSearch 2.x** | Full-text and k-NN vector search. FIBO concept facets. |
| **Apache Kafka** | Event backbone. KRaft mode. Log-compacted entity topics. |
| **MinIO** | Harvest snapshots, DDL files, DCAT exports. S3-compatible. |

---

> "The data governance tools we use today were designed for a world where *having* metadata was enough. ODIN is designed for a world where *understanding* metadata is the only thing that matters."
>
> - From the ODIN Catalog design manifesto

---

## Early Access Program

We're working with a small group of data engineering and governance teams across financial services, healthcare, and enterprise technology. Alpha participants get direct access to the core team, influence the roadmap, and help shape the product before public launch.

**[Request early access →](mailto://hi@odin-catalog.com)**

| | |
|---|---|
| **6** | Microservices, all open source |
| **6** | Open standards at the core |
| **0** | Vendor lock-in |

---

## License

Apache 2.0 · [GitHub](https://github.com/odin-catalog/odin)
