<!-- markdownlint-disable MD013 -->
<!-- markdownlint-disable MD032 -->
<!-- markdownlint-disable MD041 -->

- [What is a Data Product?](#what-is-a-data-product)
  - [Core Components of a Data Product](#core-components-of-a-data-product)
    - [Data](#data)
    - [Code and Infrastructure](#code-and-infrastructure)
    - [Metadata](#metadata)
    - [Governance](#governance)
    - [Semantics/Domain Model](#semanticsdomain-model)
    - [Access/Interfaces](#accessinterfaces)
  - [Data as a Product](#data-as-a-product)
  - [Examples of Data Products](#examples-of-data-products)

# What is a Data Product?

> A `data product` is a reusable, self-contained data asset that combines `data`, `metadata`, `semantics`, and often `code and infrastructure` to serve a specific business purpose or use case for a data consumer.
>> It is a key concept in the `Data Mesh` architectural paradigm, which treats data as a first-class product, moving away from centralized data architectures like data warehouses or data lakes.

---

## Core Components of a Data Product

A data product is more than just a dataset; it's a complete package designed for consumption.
Typical components include:

![data-product](img/data-product.drawio.svg)

### Data

> The actual data (tables, files, views, streams, etc.), which may be raw or curated (cleaned and transformed).

### Code and Infrastructure

> The necessary logic and underlying platform components to process, store, and serve the data product.

### Metadata

> Data about the data, such as column definitions, quality metrics, refresh patterns, and lineage, making the product `discoverable` and `understandable`

### Governance

> Clear ownership, quality controls, security measures, and adherence to Service Level Objectives (SLOs).

### Semantics/Domain Model

> A business-friendly layer that defines the meaning of the data, metrics, and transformation logic in business terms.

### Access/Interfaces

> Mechanisms like `APIs` or user-friendly `dashboards` that allow consumers to easily and securely access the data without needing to know the technical complexities behind it.

---

## Data as a Product

Data as a product should be:

- `Discoverable:`
    > Easily searchable in a catalog with rich documentation.

- `Addressable:`
    > Located at a permanent and unique access point.

- `Understandable:`
    > Clear semantics and documentation for easy interpretation.

- `Trustworthy:`
    > High-quality, governed data with transparency on its source and reliability.

- `Natively Accessible/Usable:`
    > Available in a format that integrates easily with consumers' tools.

- `Valuable:`
    > Delivers intrinsic value without further processing.

---

## Examples of Data Products

Data products can range from simple, well-defined datasets to complex analytical tools:

| Category | Example Data Product | Description |
| :--- | :--- | :--- |
| `Curated Datasets` | Golden Customer Record | A single, trusted dataset that consolidates and standardizes key customer information from all source systems. |
| `Insight-Based` | Sales Forecast Dashboard | A real-time dashboard or report that provides actionable insights to sales managers, including predictive analytics. |
| `Algorithms/Models` | Recommendation Engine API | A service that outputs personalized product or content recommendations based on user history and behavior. |
| `Source-Aligned` | Cleaned Transaction Log | A governed, cleaned, and standardized view of raw transactional data, optimized for analytical consumption. |
