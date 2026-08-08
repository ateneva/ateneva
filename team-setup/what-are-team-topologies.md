
# Conway's Law & Team Topologies

- [Conway's Law \& Team Topologies](#conways-law--team-topologies)
  - [What is Conway's Law?](#what-is-conways-law)
    - [How It Works in Practice](#how-it-works-in-practice)
  - [The Inverse Conway Maneuver](#the-inverse-conway-maneuver)
    - [Why Companies Use It](#why-companies-use-it)
    - [Key Challenges of the Maneuver](#key-challenges-of-the-maneuver)
    - [Real-World Tech Industry Examples](#real-world-tech-industry-examples)
      - [1. Amazon: "Two-Pizza Teams" \& Service-Oriented Architecture (2002)](#1-amazon-two-pizza-teams--service-oriented-architecture-2002)
      - [2. Netflix: Autonomous Microservices Culture](#2-netflix-autonomous-microservices-culture)
      - [3. Spotify: Squads, Tribes, and Guilds](#3-spotify-squads-tribes-and-guilds)
  - [What are Team Tolopologies?](#what-are-team-tolopologies)
    - [The 4 Fundamental Team Types](#the-4-fundamental-team-types)
    - [The 3 Interaction Modes](#the-3-interaction-modes)
      - [Collaboration](#collaboration)
      - [X-as-a-Service](#x-as-a-service)
      - [Facilitating](#facilitating)
    - [Core Principles of Team Topologies](#core-principles-of-team-topologies)
      - [Manage Cognitive Load](#manage-cognitive-load)
      - [Team-First Boundaries](#team-first-boundaries)
      - [Design for The Inverse Conway Maneuver](#design-for-the-inverse-conway-maneuver)

## What is Conway's Law?

`Conway’s Law` is an observation in software engineering and organizational management stating that *an organization's internal communication structure directly shapes the architecture of the systems it builds*

Formulated in 1967 by computer programmer Melvin Conway, the law states:

> *"Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations."*

---

### How It Works in Practice

If four separate, isolated engineering teams work together to build a compiler, you will end up with a 4-pass compiler. If you split a web application team into `Frontend, Backend, and Database teams`, your software architecture will inevitably settle into a three-tiered model with *distinct boundaries between UI, API, and DB*

The core reasons for this phenomenon include:

- *Silos mirror APIs*
  > Communication flows easily within a team, leading to tightly coupled modules inside that team's scope. Interfaces between different systems (APIs) end up reflecting the formal, cross-team communication channels.

- *Friction creates boundaries*
  > When two groups rarely talk or report to different leaders, they will naturally build isolation layers to avoid waiting on each other.

---

## The Inverse Conway Maneuver

> Because organizational structure dictates technical architecture, tech companies use the *Inverse Conway Maneuver*
>>The *Inverse Conway Maneuver* is an organizational strategy where a company *deliberately reorganizes its engineering teams and communication paths to mirror the target software architecture it wants to build*.
>>> Instead of letting existing team silos dictate system design (Conway's Law), the leadership designs the organization first so that the desired technical architecture emerges naturally.

---

### Why Companies Use It

> When software systems grow large, tightly coupled organizational structures create severe technical bottlenecks:

```plaintext
[ Traditional Setup ]                      [ Desired Target ]
Frontend Team                              ┌───────────────────┐
     │  (API contracts, handoffs)          │ Payments Service  │
Backend Team                               └───────────────────┘
     │  (Database tickets, schema blocks)  ┌───────────────────┐
Database Team                              │ Logistics Service │
                                           └───────────────────┘
```

> If a team structure looks like the left, attempting to build decoupled microservices (right) usually fails. The teams will constantly fight inter-team dependency friction, cross-boundary pull requests, and slow release cycles, eventually bending the architecture back into a distributed monolith.

To achieve microservices, team boundaries must be broken down and re-assembled into cross-functional, domain-focused units.

| Desired Software Architecture | Required Team Structure |
| --- | --- |
| *Microservices / Decoupled Systems* | Small, autonomous, cross-functional teams (e.g., Amazon's "Two-Pizza Teams") with clear domain ownership. |
| *Monolithic System* | One large, centralized team sharing a single codebase and backlog. |
| *End-to-End Product Ownership* | Teams aligned by user feature (Vertical Teams) rather than technology layer (Horizontal Teams). |

---

### Key Challenges of the Maneuver

While powerful, executing an Inverse Conway Maneuver carries significant operational risks:

- *Organizational Friction*
  > Re-architecting human relationships and management chains is far harder than refactoring code. It causes temporary productivity loss and role confusion.

- *Premature Optimization*
  > Restructuring teams around a domain model that isn't yet well understood leads to incorrect software domain boundaries that are difficult to undo.

- *Duplication of Effort*
  > High team autonomy often leads to teams re-inventing the wheel (e.g., three teams building slightly different internal caching tools) unless lightweight cross-team practices (like "Guilds" or platform engineering) exist

---

### Real-World Tech Industry Examples

#### 1. Amazon: "Two-Pizza Teams" & Service-Oriented Architecture (2002)

> In the early 2000s, Amazon operated a massive monolithic web application (`Obodos`) that blocked rapid deployment. Jeff Bezos famously issued his "API Mandate" in 2002, accompanied by a complete team restructuring

*The Restructure:*
> Amazon disbanded monolithic functional teams and created autonomous **"Two-Pizza Teams"**—groups small enough to be fed by two pizzas (roughly 6 to 10 people). Each team contained all the roles needed (engineering, product, QA, operations) to own a single, bounded context end-to-end.

*The Architectural Result:*
  > Teams were mandated to communicate *only* via exposed network interfaces (APIs). This structural shift directly birthed Amazon's highly decoupled microservices ecosystem—and eventually provided the architectural foundation for **Amazon Web Services (AWS)**.

---

#### 2. Netflix: Autonomous Microservices Culture

> As Netflix shifted from DVD shipping to streaming, its monolithic data center architecture could not scale. To build a cloud-native architecture on AWS that supported independent deployments, Netflix redesigned its entire culture and organizational chart around *"High Alignment, Low Coupling."*

*The Restructure:*
> Engineering teams were decentralized and organized around specific business domains (e.g., `Recommendations`, `Billing`, `Video Ingestion`, `Playback Engine`). Teams were empowered to choose their own tech stacks, deployment cadence, and internal tooling without centralized approval committees.

*The Architectural Result:*
> This directly shaped Netflix's microservices architecture, consisting of hundreds of microservices operating independently. If the Recommendations service fails, the Playback service still functions because the teams operate with minimal operational overlap.

---

#### 3. Spotify: Squads, Tribes, and Guilds

> As Spotify grew rapid-fire in the 2010s, they realized traditional functional departments (e.g., QA team, Backend team, Ops team) caused immense delivery lag when trying to continuously push updates to their desktop and mobile client applications.

*The Restructure:*
> created the famous *Spotify Model*

- *Squads*
    > Small, cross-functional teams that function like autonomous startups focused on a single feature area (e.g., Search, Player, Playlists).

- *Guilds:*
  > Communities of interest that cut across squads to share knowledge, best practices, and tooling (e.g., a "Backend Guild" or "Data Science Guild").
  
- *Tribes:*
  > Collections of related squads to maintain loose alignment across related domain areas.

*The Architectural Result:*
> To support squad autonomy, Spotify re-architected its desktop and mobile applications into modular micro-frontends and micro-frameworks. A squad can build, test, and release a feature directly into the production client application without needing approval from or code-merges with other squads.

---

## What are Team Tolopologies?

*Team Topologies* is a modern organizational model for software engineering and IT teams, [created by Matthew Skelton and Manuel Pais in their 2019 book Team Topologies](https://www.amazon.co.uk/Team-Topologies-Organizing-Business-Technology/dp/1942788819)

Its primary goal is to organize teams for *fast, continuous flow of value* while keeping individual team cognitive load manageable and explicitly designing around *Conway’s Law* (the idea that system architectures mirror the communication structures of the organizations that build them).

---

### The 4 Fundamental Team Types

Rather than organizing teams by function (e.g., front-end, back-end, QA, or DBA), Team Topologies defines `four well-defined team roles`:

| Team Type | Primary Purpose | Key Focus |
| --- | --- | --- |
| *Stream-Aligned* | Aligned to a single, continuous stream of work (e.g., a product feature, user journey, or business domain). | Delivering customer value quickly and autonomously without waiting on other teams. |
| *Enabling* | Composed of specialists who help stream-aligned teams adopt new skills, tools, or techniques. | Upskilling and coaching other teams so they can bridge technical gaps and remain self-sufficient. |
| *Complicated-Subsystem* | Responsible for a specific, mathematically or technically complex subsystem (e.g., video processing, AI models, billing algorithms). | Abstracting deep, specialized complexity away from stream-aligned teams. |
| *Platform* | Provides an internal, product-like platform that enables stream-aligned teams to deliver work autonomously. | Delivering "self-service" capabilities (like cloud infrastructure, CI/CD pipelines, or design systems). |

---

### The 3 Interaction Modes

> To avoid chaotic, unstructured communication across teams, Team Topologies defines `three explicit ways teams interact`

#### Collaboration

- Two teams work closely together for a defined, limited period to solve a shared problem or build a new capability.
  
> Use for high innovation, rapid discovery, or during early integration phases.

#### X-as-a-Service

- *One team provides a service, tool, or capability that another team consumes* as a self-service product with clear APIs/documentation.

> Use for high efficiency and stability; requires minimal active communication.

#### Facilitating

- One *team actively coaches or mentors another team to adopt new tools*, practices, or domain skills.

> Use when upskilling teams, clearing blockers, or adopting new industry techniques.

---

### Core Principles of Team Topologies

#### Manage Cognitive Load
  
> Teams should not be overwhelmed by too many responsibilities, systems, or domains. If cognitive load is exceeded, quality and velocity drop.

#### Team-First Boundaries

> Software architecture should be designed around small, long-lived, autonomous teams (typically 5 to 9 people), rather than forcing teams to adapt to rigid architectures

#### Design for The Inverse Conway Maneuver

> Design team structures deliberately to incentivize the target software architecture you want to achieve.
