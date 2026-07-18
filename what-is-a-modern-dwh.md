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
  - [Is Kimbal Dead in 2025?](#is-kimbal-dead-in-2025)

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

- `Decoupled Architecture:`
    > Unlike traditional systems where compute resources and data storage are tightly coupled and must be scaled together, modern data warehouses (e.g., Snowflake, Google BigQuery, AWS Redshift) separate them.

- `Business Value:`
    > This allows you to scale up compute for peak analytical loads (like month-end reporting) and scale it down/off when not needed, while data storage costs remain stable. It also enables `workload isolation`, meaning a massive batch job won't slow down the executive dashboard.

### Elastic Scalability and Concurrency

- `Scale to Zero:`
    > The architecture is elastic, meaning resources can be scaled `near-instantaneously` based on demand, and some compute resources can even scale down to zero during idle times to save cost.

- `Business Value:`
    > Handles massive, unpredictable data growth and fluctuating user concurrency (e.g., thousands of analysts running queries simultaneously) without performance degradation.

### Support for Diverse Data Types

- `Polyglot Storage:`
    > Traditional data warehouses were primarily for `structured data`. Modern MDWs can natively ingest and query `semi-structured data` (JSON, XML, logs) and integrate seamlessly with data lakes for `unstructured data` (images, video, documents).

- `Business Value:`
    > Enables comprehensive analysis across all organizational data assets, not just pre-processed relational data.

### Real-Time and Streaming Capabilities

- `Near Real-Time Analytics:`
    > While traditional warehouses relied on batch ETL that ran overnight, modern architectures support `real-time data ingestion` and analysis of streaming data (e.g., IoT device data, website clicks).

- `ELT Preference:`
    > They often favor `ELT (Extract, Load, Transform)` over ETL, loading raw data directly into the warehouse/lake before transformation, enabling faster data access and greater flexibility.

- `Business Value:`
    > Allows for timely decision-making, such as real-time fraud detection or instant adjustments based on most recent data.

### Advanced Analytics and AI/ML Integration

- `AI/ML Readiness:`
    > A good modern data warehouse is built to be the platform for machine learning (ML) and artificial intelligence (AI) projects, not just BI reporting.

- `In-Database Functions:`
    > Many modern systems include `built-in ML functions` or easy integration with data science tools (Python, R), allowing models to be trained and run directly on the data.

- `Business Value:`
    > Moves the business beyond descriptive reporting ("What happened?") to predictive and prescriptive analytics ("What will happen?" and "What should we do?").

---

## General principles when building a modern datawarehouse

- Analyst friendly
    > Analysts shouldn't have to do multiple joins to retreive the data they need

- Subject-oriented
    > It is organized around major subjects of the business, such as customers, products, orders, vouchers

- Relevant
    > Data should reflect how current underlying platform functions

- Minimal Data Processing
    > Only process pieces of information that have changed

- Ease of Maintenance
    > It should be possible to backfill historical data through the regular ETL without having to do extra code adjustments

- Avoid complex dependencies
    > Processing is decoupled by topic rather than monolitic setup of processing all topics together

- Data consistency

- Reliability

---

## Is Kimbal Dead in 2025?
