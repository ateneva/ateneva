
# What is Domain-Driven Design (DDD)?

- [What is Domain-Driven Design (DDD)?](#what-is-domain-driven-design-ddd)
  - [1. Strategic Design (The Big Picture)](#1-strategic-design-the-big-picture)
  - [2. Tactical Design (Building Blocks in Code)](#2-tactical-design-building-blocks-in-code)
  - [When to Use DDD (And When Not To)](#when-to-use-ddd-and-when-not-to)

**Domain-Driven Design (DDD)** is a [software development approach introduced by Eric Evans in his 2003 book](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software-ebook/dp/B00794TAUG/ref=sr_1_1?crid=3368VF8RP6TXA&dib=eyJ2IjoiMSJ9.8DQ78siAvZbQyEWPxae-OzxZb3b6K6spxw_6ZBSEP0eqZCi6pSArgcvUnSbYKh8gh0gxfw5lj2STaNxLU_QvqLgV0vZprXG7WOPpAYe_oHJXN58ebbZXlqIqbmVtFFYJQM520zTXW__BUJucEyEB3jyKlW4NRBq2HcwKBjuJTkSpZ9_02Fsg9lYp1_MOcrTp_yPpEHy7id8SvGMEokDdf5jQb9lta5R3famlk8eGqCk.CUINX-9jvLHg_CIrG_jxn-hHkDfuRRNy8XYjqk8-y80&dib_tag=se&keywords=Domain+driven+design&qid=1786193755&s=digital-text&sprefix=domain+driven+design%2Cdigital-text%2C148&sr=1-1)

> It focuses on modeling software around the real-world business logic and problem domain rather than technical implementations or database schemas.

The core philosophy is simple: **the structure and language of software code should directly match the business domain.**

---

DDD breaks down into two main pillars: **Strategic Design** (high-level architecture and organizational alignment) and **Tactical Design** (code-level implementation patterns).

## 1. Strategic Design (The Big Picture)

Strategic design helps teams map out complex systems, define team boundaries, and align developers with business experts.

- **Ubiquitous Language:**
  > A single shared vocabulary used by both domain experts (business analysts, product owners) and technical builders (developers). The same terms used in business meetings must appear directly as classes, variables, and methods in the code.

- **Bounded Context:**
  > Explicit boundaries within which a domain model applies. For instance, the word "Account" means something very different in a *Billing Context* (invoices, payments) vs. a *Shipping Context* (address, tracking history).

- **Context Mapping:**
  > A diagram or definition showing how different bounded contexts interact and exchange data (e.g., via APIs, messaging queues, or customer-supplier relationships).

- **Subdomains:**
  > Dividing the domain into **Core** (your primary competitive advantage), **Supporting** (necessary for business but not unique), and **Generic** (standard problems like user authentication or payment gateways that could be outsourced or bought off-the-shelf).

---

## 2. Tactical Design (Building Blocks in Code)

Tactical patterns provide concrete software engineering tools to implement the domain model securely and expressively inside a bounded context.

| Building Block | Description | Example |
| --- | --- | --- |
| **Entities** | Objects with a unique identity that persists over time, regardless of property changes. | A `User` with an ID, or an `Order` tracked across states. |
| **Value Objects** | Immutable objects defined entirely by their attributes, without a conceptual identity. | `Money(amount=10, currency="USD")` or an `Address`. |
| **Aggregates** | A cluster of associated Entities and Value Objects treated as a single unit for data changes. | An `Order` aggregate containing `OrderItem` entities. |
| **Aggregate Root** | The single primary Entity in an Aggregate that controls all access and enforces business rules. | You modify `OrderItem` only through the `Order` root. |
| **Repositories** | Abstraction layers that mimic in-memory collections to load and persist Aggregate Roots. | `OrderRepository.findById(id)` hiding SQL or database logic. |
| **Domain Events** | Notifications that record something significant happening in the domain. | `OrderPlacedEvent` or `PaymentFailedEvent`. |

---

## When to Use DDD (And When Not To)

DDD is not a silver bullet and adds initial design overhead.

- **Best suited for:**
  > Complex business logic, large enterprise platforms, microservice architectures, and long-term systems where domain knowledge is intricate.

- **Avoid for:**
  > Simple CRUD applications, basic utilities, data pipelines, or short-lived prototypes where business rules are minimal
