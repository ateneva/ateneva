<!-- markdownlint-disable MD013 -->
<!-- markdownlint-disable MD032 -->
<!-- markdownlint-disable MD041 -->

- [What are the hallmarks of a good modern datawarehouse?](#what-are-the-hallmarks-of-a-good-modern-datawarehouse)
  - [The Core Pillars of a Modern Data Warehouse](#the-core-pillars-of-a-modern-data-warehouse)
    - [Separation of Compute \& Storage](#separation-of-compute--storage)
    - [Elastic Scalability and Concurrency](#elastic-scalability-and-concurrency)
    - [Support for Diverse Data Types](#support-for-diverse-data-types)
    - [Real-Time and Streaming Capabilities](#real-time-and-streaming-capabilities)
    - [Advanced Analytics and AI/ML Integration](#advanced-analytics-and-aiml-integration)
  - [General principles when building a modern datawarehouse](#general-principles-when-building-a-modern-datawarehouse)
- [Data Modelling Approaches](#data-modelling-approaches)
  - [Kimball Dimensional Modeling vs One Big Table (OBT)](#kimball-dimensional-modeling-vs-one-big-table-obt)
    - [Core Comparison](#core-comparison)
    - [Deep Dive: Architectural Trade-Offs](#deep-dive-architectural-trade-offs)
      - [1. Kimball Dimensional Modeling](#1-kimball-dimensional-modeling)
      - [2. One Big Table (OBT)](#2-one-big-table-obt)
    - [When to Choose Which](#when-to-choose-which)
  - [Data Modelling Strategies under Data Mesh](#data-modelling-strategies-under-data-mesh)
    - [1. Federated Computational Governance \& Interoperability](#1-federated-computational-governance--interoperability)
    - [2. Domain Ownership \& Clear Boundaries](#2-domain-ownership--clear-boundaries)
    - [3. Data as a Product (Change Management)](#3-data-as-a-product-change-management)
    - [The Operational Caveat: Where OBT Fits in Data Mesh](#the-operational-caveat-where-obt-fits-in-data-mesh)
    - [Decision Matrix for Data Mesh](#decision-matrix-for-data-mesh)
  - [Data Modelling Strategies under Data Mresh and Domain-driven Design (DDD)](#data-modelling-strategies-under-data-mresh-and-domain-driven-design-ddd)
    - [How Domain-driven Design Principles Shift the Modeling Landscape](#how-domain-driven-design-principles-shift-the-modeling-landscape)
      - [1. Bounded Contexts vs. Enterprise Conformed Dimensions](#1-bounded-contexts-vs-enterprise-conformed-dimensions)
      - [2. Domain Events as the Primary Source of Truth](#2-domain-events-as-the-primary-source-of-truth)
    - [Comparing the Options in DDD + Data Mesh](#comparing-the-options-in-ddd--data-mesh)
      - [Option 1: Strictly OBT per Bounded Context (Event-Centric)](#option-1-strictly-obt-per-bounded-context-event-centric)
      - [Option 2: Internal Kimball, Exposed OBT (Hybrid Domain Product)](#option-2-internal-kimball-exposed-obt-hybrid-domain-product)
    - [The Verdict: How to Model for DDD + Data Mesh](#the-verdict-how-to-model-for-ddd--data-mesh)
    - [Rule of Thumb](#rule-of-thumb)
    - [Practical Example: Option 1 in Action](#practical-example-option-1-in-action)
      - [Domain 1: Checkout \& Payments Bounded Context](#domain-1-checkout--payments-bounded-context)
        - [1. Incoming Operational Domain Event Stream (`OrderCompleted`)](#1-incoming-operational-domain-event-stream-ordercompleted)
        - [2. Materialized OBT Data Product (`checkout.data_product_orders_obt`)](#2-materialized-obt-data-product-checkoutdata_product_orders_obt)
      - [Domain 2: Warehouse \& Fulfillment Bounded Context](#domain-2-warehouse--fulfillment-bounded-context)
        - [1. Incoming Operational Domain Event Stream (`PackageDispatched`)](#1-incoming-operational-domain-event-stream-packagedispatched)
        - [2. Materialized OBT Data Product (`fulfillment.data_product_dispatch_obt`)](#2-materialized-obt-data-product-fulfillmentdata_product_dispatch_obt)
      - [How Mesh Consumers Query Option 1 Data Products](#how-mesh-consumers-query-option-1-data-products)
      - [Why Option 1 Works Well in DDD Data Mesh](#why-option-1-works-well-in-ddd-data-mesh)
- [OBT Maintenance \& Operational Strategies](#obt-maintenance--operational-strategies)
  - [1. Handling Schema Evolution](#1-handling-schema-evolution)
    - [Strategy: Additive-Only Changes \& Semantic Versioning](#strategy-additive-only-changes--semantic-versioning)
  - [2. Handling Late-Arriving Events](#2-handling-late-arriving-events)
    - [Strategy: Two-Timestamp Architecture \& Append-Only Ingestion](#strategy-two-timestamp-architecture--append-only-ingestion)
  - [3. Handling Backfills \& Retrospective Updates](#3-handling-backfills--retrospective-updates)
    - [Strategy: Immutable Re-Processing via Storage Table Formats](#strategy-immutable-re-processing-via-storage-table-formats)
  - [Strategy Summary](#strategy-summary)

---

# What are the hallmarks of a good modern datawarehouse?

> A good modern data warehouse is a `flexible, cloud-native architecture` that prioritizes:
> - `speed`
> - `scalability`
> - `support for advanced analytics`

---

## The Core Pillars of a Modern Data Warehouse

> The most significant shift is the separation of compute and storage in a cloud environment

### Separation of Compute & Storage

- *Decoupled Architecture*
    > Unlike traditional systems where compute resources and data storage are tightly coupled and must be scaled together, modern data warehouses (e.g., Snowflake, Google BigQuery, AWS Redshift) separate them.

- *Business Value*
    > This allows you to scale up compute for peak analytical loads (like month-end reporting) and scale it down/off when not needed, while data storage costs remain stable. It also enables `workload isolation`, meaning a massive batch job won't slow down the executive dashboard.

### Elastic Scalability and Concurrency

- *Scale to Zero*
    > The architecture is elastic, meaning resources can be scaled `near-instantaneously` based on demand, and some compute resources can even scale down to zero during idle times to save cost.

- *Business Value*
    > Handles massive, unpredictable data growth and fluctuating user concurrency (e.g., thousands of analysts running queries simultaneously) without performance degradation.

### Support for Diverse Data Types

- *Polyglot Storage*
    > Traditional data warehouses were primarily for `structured data`. Modern MDWs can natively ingest and query `semi-structured data` (JSON, XML, logs) and integrate seamlessly with data lakes for `unstructured data` (images, video, documents).

- *Business Value*
    > Enables comprehensive analysis across all organizational data assets, not just pre-processed relational data.

### Real-Time and Streaming Capabilities

- *Near Real-Time Analytics*
    > While traditional warehouses relied on batch ETL that ran overnight, modern architectures support `real-time data ingestion` and analysis of streaming data (e.g., IoT device data, website clicks).

- *ELT Preference*
    > They often favor `ELT (Extract, Load, Transform)` over ETL, loading raw data directly into the warehouse/lake before transformation, enabling faster data access and greater flexibility.

- *Business Value*
    > Allows for timely decision-making, such as real-time fraud detection or instant adjustments based on most recent data.

### Advanced Analytics and AI/ML Integration

- *AI/ML Readiness*
    > A good modern data warehouse is built to be the platform for machine learning (ML) and artificial intelligence (AI) projects, not just BI reporting.

- *In-Database Functions*
    > Many modern systems include `built-in ML functions` or easy integration with data science tools (Python, R), allowing models to be trained and run directly on the data.

- *Business Value*
    > Moves the business beyond descriptive reporting ("What happened?") to predictive and prescriptive analytics ("What will happen?" and "What should we do?").

---

## General principles when building a modern datawarehouse

- *Analyst friendly*
    > Analysts shouldn't have to do multiple joins to retreive the data they need

- *Subject-oriented*
    > It is organized around major subjects of the business, such as customers, products, orders, vouchers

- *Relevant*
    > Data should reflect how current underlying platform functions

- *Minimal Data Processing*
    > Only process pieces of information that have changed

- *Ease of Maintenance*
    > It should be possible to backfill historical data through the regular ETL without having to do extra code adjustments

- *Avoid complex dependencies*
    > Processing is decoupled by topic rather than monolitic setup of processing all topics together

- *Data consistency*
  > Extensive data quality checks and monitoring patterns should be in place to ensure that the data is consistent across all topics

- *Reliability*
  > The data warehouse should be resilient to failures, with mechanisms in place for retries, alerting and quick-time-to-recovery in case of failures

---

# Data Modelling Approaches

## Kimball Dimensional Modeling vs One Big Table (OBT)

> *Kimball Dimensional Modeling* separates data into facts (numerical metrics) and dimensions (contextual attributes) joined in a star schema, whereas *One Big Table (OBT)* consolidates all facts and dimensions into a single, fully denormalized wide table.
>> While Kimball was designed to minimize storage redundancy and streamline complex relational reporting, OBT leverages the massive parallel processing (MPP) and columnar compression of modern cloud data warehouses to eliminate costly runtime JOINs.

---

### Core Comparison

| Feature | Kimball Dimensional Modeling | One Big Table (OBT) |
| --- | --- | --- |
| *Structure* | Star schema (Fact tables surrounded by Dimension tables) | Single flat, wide table (often 100+ columns) |
| *Data Redundancy* | Low (attributes stored once in dimension tables) | High (attributes repeated across millions of rows) |
| *Query Complexity* | Moderate to High (requires `JOIN` clauses between facts and dimensions) | Low (simple `SELECT ... WHERE ... GROUP BY` with no joins) |
| *Query Performance* | Fast for small datasets; can slow down on massive join operations | Blazing fast for aggregations in MPP columnar engines (BigQuery, Snowflake, Databricks) |
| *ETL / Data Engineering* | High initial complexity (managing surrogate keys, Slowly Changing Dimensions) | Lower pipeline complexity (flat transformations, wide `JOIN` at build time via dbt) |
| *Storage Efficiency* | Higher structural efficiency (normalized) | Relies on columnar compression (Parquet/ORC) to mitigate redundancy cost |
| *Business User Usability* | Requires understanding of schema relationships and join logic | Highly intuitive for self-service BI and non-technical analysts |
| *Data Governance & Updates* | Easy to update context (update one dimension row; all facts reflect it) | Harder to update (updating a dimension attribute requires updating millions of rows) |

---

### Deep Dive: Architectural Trade-Offs

#### 1. Kimball Dimensional Modeling

- *How it works:* Structured around business processes. A *Fact table* holds quantitative measurements (e.g., `revenue`, `quantity_sold`) and foreign keys referencing *Dimension tables* (e.g., `customer`, `product`, `store`, `date`).

**Pros:**
- *Conformed Dimensions:* Ensures consistent reporting across different business processes via a central bus matrix.

- *Scalable Data Management:* Handles Slowly Changing Dimensions (SCD Type 1, Type 2) cleanly without rewriting billions of fact records.

- *Storage Economy:* Eliminates string duplicate values across millions of rows.

**Cons:**

- *Join Overhead:* Analytical queries require multi-table `JOIN` operations, which can become expensive at petabyte scale.

- *Steep Learning Curve:* Data teams must carefully design enterprise data warehouse architectures and maintain surrogate key pipelines.

---

#### 2. One Big Table (OBT)

- *How it works:* Denormalizes all dimensional context directly into the event record. Every row contains both the metric and every piece of surrounding attribute data.

**Pros:**

- *No Runtime Joins:* Eliminates `JOIN` execution costs and potential join-fanout bugs during query runtime.

- *Columnar Power:* Cloud data warehouses (Snowflake, BigQuery, ClickHouse) only scan the specific columns requested in a `SELECT` statement, making OBT queries extremely fast and cost-effective for BI tools (e.g., Tableau, PowerBI, Looker).

- *Self-Service Friendly:* Analysts and BI tools don't need complex logical models—they query a single table.

**Cons:**

- *Mutation Costs:* Changing a single attribute (e.g., a customer changing their home state) requires updating or rebuilding massive tables.

- *Data Drift Risk:* Without centralized dimension tables, string mismatches or inconsistent attribute definitions can creep into different OBT builds.

- *Compute Cost at Build Time:* Re-building wide tables downstream in dbt on high-frequency schedules can incur significant materialization costs.

---

### When to Choose Which

*Choose Kimball if:*
- You have complex enterprise dimensions shared across multiple facts (e.g., a single `Customer` dimension used by Sales, Support, and Marketing).

- Your organization frequently tracks historical dimension changes (SCD Type 2).

- You want tight governance, low data duplication, and a unified semantic layer across all business units.

*Choose OBT if:*
- You rely on modern cloud columnar engines (Snowflake, BigQuery, Databricks) and want maximum read performance for dashboards.

- Your primary consumers are BI users or ad-hoc data analysts who benefit from simplified SQL queries.

- You use automated transformation frameworks like *dbt* where you can maintain Kimball modeling in your *marts/intermediate* layer, but materialize OBTs for final reporting consumption.

*Modern Hybrid Pattern:*
> Most modern data stacks do not force a strict choice. Data engineering teams often maintain a normalized *Kimball star schema* in the transformation layer to enforce governance and manage SCDs, then materialize *OBT analytical views* on top for BI tools and fast ad-hoc querying.

## Data Modelling Strategies under Data Mesh

### 1. Federated Computational Governance & Interoperability

- *Kimball*
  > Relies on **Conformed Dimensions** (e.g., standard definitions for `Customer`, `Product`, or `Location` shared across domains). In a Data Mesh, conformed dimensions act as the "interoperable glue" that allows cross-domain queries. A *Sales Domain* fact table and a *Support Domain* fact table can be joined seamlessly because both reference the same conformed `Customer` dimension.

- *OBT*
  > Highly isolated. If every domain builds its own OBT, attributes like `Customer_Address` or `Product_Category` get hardcoded directly into wide tables. Over time, logic drifts, leading to inconsistent definitions across domains and making cross-domain analytics nearly impossible without massive re-engineering.

### 2. Domain Ownership & Clear Boundaries

- *Kimball*
  > Clearly separates business metrics (*Facts*) from contextual entities (*Dimensions*). Domains can easily own specific entities (e.g., the *CRM Domain* owns `dim_customer`; the *E-commerce Domain* owns `fact_orders`) and expose them as clean data interfaces.

- *OBT*
  > Blurs domain boundaries by requiring pre-joins of foreign domain context at build time. If the *Order Domain* needs to publish an OBT, it must absorb data from the *Customer*, *Shipping*, and *Inventory* domains into a single table. This forces cross-domain coupling during pipeline transformation.

### 3. Data as a Product (Change Management)

- *Kimball*
  > Gracefully handles business changes using Slowly Changing Dimensions (SCD Type 2). If a customer updates their regional address, the change is recorded once in `dim_customer` without rewriting petabytes of historical sales events.

- *OBT*
  > Updating a single dimension attribute forces a domain team to rewrite millions or billions of rows in their wide historical tables, inflating pipeline execution costs and increasing operational overhead.

---

### The Operational Caveat: Where OBT Fits in Data Mesh

> While OBT is inadequate as an *internal data architecture* for domain products, it shines as an output for analysts who need fast, pre-joined datasets to drive dashboards and insights. In a Data Mesh, OBT can be used as a *specialized analytical layer* on top of the core Kimball star schema.

Modern Vizualization layer tools (Tableau, Power BI, Looker) and non-technical analysts often prefer flat, no-join datasets. Therefore, domain teams in a Data Mesh frequently adopt a *tiered modeling lifecycle*:

```plaintext
[ Domain Source Data ] 
       │
       ▼
[ Kimball Star Schema ]  <── Core Data Product Interface (Governed, Conformed, & Versioned)
       │
       ▼
[ OBT Materialized View ] <── Specialized Read Port (Pre-joined for fast BI dashboards)

```

---

### Decision Matrix for Data Mesh

| Criteria | Kimball Dimensional Model | One Big Table (OBT) |
| --- | --- | --- |
| *Cross-Domain Joinability* | **Best:** Shared dimensions enable cross-domain analysis. | **Poor:** Data is locked into siloed wide tables. |
| *Governance & Standardization* | **High:** Centralized/Federated definition of core entities. | **Low:** High risk of data logic drifting between teams. |
| *Pipeline Autonomy* | **High:** Domains own specific facts or dimensions independently. | **Medium:** Broad dependencies required to pre-join wide tables. |
| *BI User Usability* | **Moderate:** Requires understanding joins and relationships. | **Best:** Plug-and-play for simple drag-and-drop reporting. |

---

## Data Modelling Strategies under Data Mresh and Domain-driven Design (DDD)

> When an organization couples *Data Mesh* with *Domain-Driven Design (DDD)*, the architectural equation changes significantly. DDD provides the analytical framework to establish *Bounded Contexts*, *Aggregates*, and *Ubiquitous Language*, while Data Mesh applies these software engineering principles to data platforms.

In this combined framework, *classic enterprise-wide Kimball patterns break down across boundaries* while OBT, on the other hand, aligns tightly with event-driven domain boundaries.

---

### How Domain-driven Design Principles Shift the Modeling Landscape

#### 1. Bounded Contexts vs. Enterprise Conformed Dimensions

- *The Kimball Conflict*
  > Classic Kimball relies on *Enterprise Conformed Dimensions*—a single, globally standardized definition of entities like `Customer` or `Product`. In DDD, global schemas violate the *Bounded Context*. A `Customer` in the *Sales Context* (focused on leads, contracts, ARR) is fundamentally a different entity than a `Customer` in the *Fulfillment Context* (focused on shipping addresses, delivery instructions, carrier restrictions). Forcing a single Kimball `dim_customer` creates tight coupling across domains.

- *The DDD Reality*
  > Each domain models its own aggregates inside its bounded context.

#### 2. Domain Events as the Primary Source of Truth

- In DDD-driven systems, domains communicate asynchronously via **Domain Events** (e.g., `OrderPlaced`, `ShipmentDispatched`, `PaymentProcessed`).

- Domain events are immutable, point-in-time facts that natively fit a flat structure. An event payload published by an operational bounded context naturally closely resembles an **OBT record** for that specific point in time.

---

### Comparing the Options in DDD + Data Mesh

```plaintext
+-----------------------------------------------------------------------------------+
|                                SALES BOUNDED CONTEXT                              |
|                                                                                   |
|   Operational Domain                 Analytical Data Product                      |
|  +-------------------+              +-------------------------------+             |
|  | Sales Aggregates  | ──Events──>  |  Option A: Internal Kimball   |             |
|  |  (OLTP DB)        |              |  (Fact_Sales + Dim_Promos)    |             |
|  +-------------------+              +---------------+---------------+             |
|                                                     |                             |
|                                                     ▼                             |
|                                     |  Option B: Public OBT Port    |             |
|                                     |  (Published to Data Mesh)     |             |
|                                     +-------------------------------+             |
+-----------------------------------------------------------------------------------+
```

#### Option 1: Strictly OBT per Bounded Context (Event-Centric)

- *How it works*
  > Each domain exposes its analytical data products as flat, denormalized OBTs derived directly from its domain events and operational stores. The *Sales Domain* publishes `sales_orders_obt`, fully self-contained with all attributes relevant *only* to the Sales context.

**Pros:**
- *Zero Domain Coupling*
  > Sales never has to wait on Customer or Logistics to publish or update dimensions.

- *Immutable Audit Trail*
  > Naturally preserves historical state at the exact moment a domain event occurred without needing complex Kimball SCD Type 2 tracking.

- *Strict DDD Alignment*
  > Honors the Bounded Context completely.

**Cons:**
- *Cross-Domain Stitching Friction*
  > To answer "What is the total revenue per customer segment across Sales, Support, and Marketing?", downstream consumers must join multiple domain OBTs using a *Global Domain Key* (e.g., `customer_id`), handling schema discrepancies manually.

---

#### Option 2: Internal Kimball, Exposed OBT (Hybrid Domain Product)

- *How it works*
  > *Inside* a single domain's boundary, the domain team uses Kimball techniques to organize internal complex data (e.g., separating internal domain facts from internal domain lookup tables). However, the domain *publishes its public Data Product as an OBT interface* for mesh consumers.

**Pros:**
- Clean internal domain state management for the data engineers.
- High-performance, zero-join consumption layer for mesh consumers.

**Cons:**
- Requires maintaining two physical layers within the domain's data pipeline.

---

### The Verdict: How to Model for DDD + Data Mesh

| Layer / Level | Recommended Approach | DDD / Data Mesh Rationale |
| --- | --- | --- |
| *Across Domains (Mesh Level)* | **OBT / Immutable Event Streams** | Preserves Bounded Context boundaries. Domains expose high-performing, self-contained data products without cross-domain join dependencies. |
| *Inside a Domain (Internal Model)* | **Kimball Star Schema (Domain-Scoped)** | If a domain's internal logic is complex, Kimball works well *within* the context boundary (e.g., `fact_orders` joined to `dim_promotions` owned entirely by the Sales domain). |
| *Cross-Domain Interoperability* | **Global Entity Identifiers (Keys Only)** | Instead of conforming full dimensions enterprise-wide, federated governance enforces standard entity **IDs** (e.g., global `UUID` for `customer_id`) so mesh consumers can join domain products when necessary. |

### Rule of Thumb

In a true DDD + Data Mesh architecture, *OBT (or flat event-based tables) becomes the dominant pattern for *published Data Products*. 
> Abandon the attempt to build monolithic, enterprise-wide Kimball conformed dimensions. Keep Kimball strictly as an internal implementation detail *inside* domain boundaries where multi-entity relational logic is too complex for a flat structure.

- In an *Option 1 (Strictly OBT per Bounded Context)* architecture, there are no internal Kimball star schemas, surrogate keys, or dimension tables anywhere inside the domain.

> Instead, operational microservices publish *immutable Domain Events*. The domain data engine consumes these event streams, denormalizes them at ingestion time, and materializes a *single, flat OBT Data Product* that reflects the exact state of the domain at the moment each event occurred.

Here is how two distinct domains (**Checkout & Payments** vs. **Warehouse & Fulfillment**) implement Option 1.

---

### Practical Example: Option 1 in Action

#### Domain 1: Checkout & Payments Bounded Context

##### 1. Incoming Operational Domain Event Stream (`OrderCompleted`)

When a customer purchases items, the microservice emits an event into a message bus (Kafka/Kinesis). The payload contains all operational metadata at that exact timestamp:

```json
{
  "event_id": "evt_998124",
  "event_type": "OrderCompleted",
  "timestamp": "2026-08-09T12:15:00Z",
  "payload": {
    "order_id": "ord_5501",
    "global_customer_id": "usr_88319a2",
    "customer_snapshot": {
      "email": "alex@example.com",
      "membership_tier": "VIP_Gold",
      "acquisition_channel": "Google_Search"
    },
    "payment_details": {
      "payment_method": "Apple_Pay",
      "currency": "EUR",
      "total_amount": 149.99,
      "tax_amount": 26.03
    },
    "items_count": 2
  }
}
```

##### 2. Materialized OBT Data Product (`checkout.data_product_orders_obt`)

The Checkout domain processes this event directly into its analytical output port as a flat, single-table record. No `JOIN` against a customer or payment table ever occurs.

```sql
CREATE TABLE checkout_domain.data_product_orders_obt (
    -- Event & Transaction Identifiers
    event_id VARCHAR(64) PRIMARY KEY,
    order_id VARCHAR(64) NOT NULL,
    event_timestamp TIMESTAMP NOT NULL,

    -- Federated Global Key (For cross-domain mesh querying)
    global_customer_id UUID NOT NULL,

    -- Denormalized Customer Snapshot (Point-in-time state from event)
    customer_email VARCHAR(255),
    customer_membership_tier VARCHAR(50),
    customer_acquisition_channel VARCHAR(50),

    -- Denormalized Payment Details
    payment_method VARCHAR(50),
    currency VARCHAR(3),
    total_amount DECIMAL(12,2),
    tax_amount DECIMAL(12,2),
    items_count INT
);
```

---

#### Domain 2: Warehouse & Fulfillment Bounded Context

##### 1. Incoming Operational Domain Event Stream (`PackageDispatched`)

When the logistics system processes the order, it emits its own domain event. This event knows nothing about membership tiers or payment methods—it operates strictly within the Fulfillment bounded context:

```json
{
  "event_id": "evt_772109",
  "event_type": "PackageDispatched",
  "timestamp": "2026-08-09T14:30:00Z",
  "payload": {
    "fulfillment_id": "ful_3019",
    "order_id": "ord_5501",
    "global_customer_id": "usr_88319a2",
    "warehouse_location": "NL-Rotterdam-01",
    "carrier_details": {
      "carrier": "DHL_Express",
      "service_level": "Next_Day_Air",
      "tracking_number": "3S123456789"
    },
    "package_specs": {
      "weight_kg": 2.45,
      "dimensions_cm": "30x20x15"
    }
  }
}
```

##### 2. Materialized OBT Data Product (`fulfillment.data_product_dispatch_obt`)

The Fulfillment domain streams this event directly into its flat OBT data product:

```sql
CREATE TABLE fulfillment_domain.data_product_dispatch_obt (
    -- Event & Operational Identifiers
    event_id VARCHAR(64) PRIMARY KEY,
    fulfillment_id VARCHAR(64) NOT NULL,
    order_id VARCHAR(64) NOT NULL,
    dispatch_timestamp TIMESTAMP NOT NULL,

    -- Federated Global Keys
    global_customer_id UUID NOT NULL,

    -- Denormalized Warehouse Context
    warehouse_code VARCHAR(50),
    
    -- Denormalized Carrier & Specs
    shipping_carrier VARCHAR(50),
    service_level VARCHAR(50),
    package_weight_kg DECIMAL(6,2),
    package_dimensions_cm VARCHAR(20)
);
```

---

#### How Mesh Consumers Query Option 1 Data Products

Because both domains strictly published flat OBTs, there are **no cross-domain dependencies or shared Kimball dimensions**.

If a Central Business Intelligence team needs to analyze **whether VIP customers experience faster warehouse dispatch times**, they query both domain OBTs using the shared identifiers (`order_id` and `global_customer_id`):

```sql
SELECT 
    chk.customer_membership_tier,
    ful.shipping_carrier,
    ful.warehouse_code,
    -- Compute operational latency between domains
    AVG(TIMESTAMPDIFF(MINUTE, chk.event_timestamp, ful.dispatch_timestamp)) AS avg_dispatch_delay_minutes,
    COUNT(DISTINCT chk.order_id) AS total_orders
FROM `mesh_catalog.checkout.orders_obt` chk
JOIN `mesh_catalog.fulfillment.dispatch_obt` ful
  ON chk.order_id = ful.order_id 
 AND chk.global_customer_id = ful.global_customer_id
WHERE chk.event_timestamp >= '2026-08-01'
GROUP BY 1, 2, 3;
```

---

#### Why Option 1 Works Well in DDD Data Mesh

1. *Complete Domain Isolation*
   > The Checkout team can alter their data product schema without notifying or breaking the Fulfillment team's pipelines.

2. *Immutable History*
   > If Alex updates their tier from `VIP_Gold` to `Platinum` tomorrow, historical rows in `orders_obt` remain `VIP_Gold`. The event snapshot captures reality *at the time of the order*, eliminating the need for Kimball SCD Type 2 logic.

3. *Low Pipeline Overhead*
   > No complex multi-stage dbt models joining star schemas internally. Events stream in and land directly in wide, compressed columnar tables.

---

# OBT Maintenance & Operational Strategies

> Because context (e.g., customer tier, product price) is hardcoded into every row at ingestion, operational changes require specific strategies for backfills, schema evolution, and late-arriving events.

## 1. Handling Schema Evolution

> When a domain microservice adds, renames, or drops fields in its event payload, the published OBT Data Product must evolve without breaking downstream mesh consumers.

### Strategy: Additive-Only Changes & Semantic Versioning

- *Adding Fields (Non-Breaking)*
  > Always allow **nullability**. When a domain introduces a new attribute (e.g., `payment_provider_transaction_id`), add the column to the OBT schema with `DEFAULT NULL`. Historical rows simply remain `NULL`.

- *Breaking Changes (Renaming/Removing Fields)*
  > Do not modify the existing OBT. Instead, issue a **major version increment** on the Data Product interface (e.g., transition `checkout.orders_obt_v1` to `checkout.orders_obt_v2`).

- *Dual-Writing during Migration*
  > Run both versions in parallel for a set deprecation period (e.g., 90 days), allowing downstream consumers time to update their queries to `v2`.

```sql
-- Schema Evolution: Evolving from v1 to v2 via a view or physical table
CREATE OR REPLACE TABLE checkout_domain.orders_obt_v2 AS
SELECT 
    *,
    -- New field added in v2
    JSON_EXTRACT_SCALAR(event_payload, '$.payment_details.fraud_score') AS fraud_score
FROM checkout_domain.raw_order_events;
```

---

## 2. Handling Late-Arriving Events

> A late-arriving event occurs when an operational system emits an event out of sequence due to network partitions, offline app usage, or batch sync delays (e.g., a `PackageDispatched` event occurring at 10:00 AM arrives at 4:00 PM).

### Strategy: Two-Timestamp Architecture & Append-Only Ingestion

> Because OBT tables in modern data lakehouses (e.g., Iceberg, Delta Lake, BigQuery) are optimized for append-only writes, *never attempt to retroactively insert or update historical micro-batches*. Instead:

1. *Maintain Two Timestamps on Every Row*
   - `event_timestamp`: When the real-world business action occurred.
   - `ingestion_timestamp`: When the record was appended to the OBT.

2. *Partition by Ingestion Date, Cluster/Sort by Event Date:* 
   > Partitioning by `ingestion_timestamp` allows low-cost append streaming. Sorting or clustering by `event_timestamp` keeps time-series range queries fast for BI consumers.

3. *Consumer Query Standard:*
   > Ensure downstream consumers filter by `event_timestamp` for business logic (e.g., monthly sales reports) while using `ingestion_timestamp` for incremental ETL processing.

```sql
CREATE TABLE fulfillment_domain.dispatch_obt (
    event_id STRING,
    order_id STRING,
    event_timestamp TIMESTAMP,      -- Used for business reporting
    ingestion_timestamp TIMESTAMP,  -- Used for data pipeline tracking
    ...
)
PARTITION BY DATE(ingestion_timestamp)
CLUSTER BY event_timestamp, order_id;
```

---

## 3. Handling Backfills & Retrospective Updates

> Backfills are the most challenging part of an OBT architecture. If business logic changes (e.g., recalculating a computed field across two years of data) or an operational bug corrupts past events, you cannot simply update a single dimension row as you would in Kimball. You must update millions of wide OBT rows.

### Strategy: Immutable Re-Processing via Storage Table Formats

Leverage modern open table formats (*Apache Iceberg*, *Delta Lake*) or modern dbt incremental strategies to recompute partitions in bulk.

```plaintext
+-----------------------------------------------------------------------+
|                         RE-PROCESSING PIPELINE                        |
|                                                                       |
|  [ Raw Immutable Event Store ] (Kafka / S3 Event Logs)                |
|               │                                                       |
|               ▼                                                       |
|  [ Updated Transformation Logic ] (dbt / Spark / Flink)               |
|               │                                                       |
|               ▼                                                       |
|  [ Write to Staging OBT Table ]                                       |
|               │                                                       |
|               ▼                                                       |
|  [ Atomic Partition Swap / Iceberg Branch Replace ] ──> [ Live OBT ]  |
+-----------------------------------------------------------------------+
```

1. *Replay from Raw Event Logs* Domains should retain immutable raw event payloads in cheap cloud storage (S3/GCS) or long-term Kafka/Pulsar topics.

2. *Atomic Partition Swaps* Run a batch job (via Spark, Flink, or dbt) that reads the raw immutable events, applies the updated transformation, writes to a staging table, and executes an atomic partition swap into OBT.

```sql
-- Atomic replacement of specific month partition using Apache Iceberg syntax
INSERT OVERWRITE checkout_domain.orders_obt
SELECT 
    event_id,
    order_id,
    event_timestamp,
    ...
FROM checkout_domain.staging_orders_obt_backfill
WHERE event_timestamp BETWEEN '2026-01-01' AND '2026-01-31';
```

---

## Strategy Summary

| Challenge | Recommended Mechanism | Key Benefit |
| --- | --- | --- |
| *Schema Evolution* | Additive columns with `NULL` defaults; Semantic Versioning (`v1`, `v2`) for breaking changes. | Zero downtime; doesn't break active mesh consumers. |
| *Late-Arriving Events* | Partition by `ingestion_timestamp`, cluster by `event_timestamp`. Append-only streaming. | Avoids expensive merge/update mutations on wide tables. |
| *Historical Backfills* | Replay raw event logs using atomic table/partition swaps (Iceberg / Delta Lake). | Ensures complete data lineage consistency without locking the production OBT. |
