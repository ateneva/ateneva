
# What are Team Topologies?

- [What are Team Topologies?](#what-are-team-topologies)
  - [The 4 Fundamental Team Types](#the-4-fundamental-team-types)
  - [The 3 Interaction Modes](#the-3-interaction-modes)
    - [Collaboration](#collaboration)
    - [X-as-a-Service](#x-as-a-service)
    - [Facilitating](#facilitating)
  - [Core Operating Principles](#core-operating-principles)
    - [Manage Cognitive Load](#manage-cognitive-load)
    - [Team-First Boundaries](#team-first-boundaries)
    - [The Inverse Conway Maneuver](#the-inverse-conway-maneuver)

`Team Topologies` is a modern organizational model for software engineering and IT teams, created by `Matthew Skelton` and `Manuel Pais` in their 2019 book [Team Topologies](https://www.amazon.co.uk/Team-Topologies-Organizing-Business-Technology/dp/1942788819)

Its primary goal is to organize teams for `fast, continuous flow of value` while keeping individual team cognitive load manageable and explicitly designing around `Conway’s Law` (the idea that system architectures mirror the communication structures of the organizations that build them).

---

## The 4 Fundamental Team Types

Rather than organizing teams by function (e.g., front-end, back-end, QA, or DBA), Team Topologies defines `four well-defined team roles`:

| Team Type | Primary Purpose | Key Focus |
| --- | --- | --- |
| `Stream-Aligned` | Aligned to a single, continuous stream of work (e.g., a product feature, user journey, or business domain). | Delivering customer value quickly and autonomously without waiting on other teams. |
| `Enabling` | Composed of specialists who help stream-aligned teams adopt new skills, tools, or techniques. | Upskilling and coaching other teams so they can bridge technical gaps and remain self-sufficient. |
| `Complicated-Subsystem` | Responsible for a specific, mathematically or technically complex subsystem (e.g., video processing, AI models, billing algorithms). | Abstracting deep, specialized complexity away from stream-aligned teams. |
| `Platform` | Provides an internal, product-like platform that enables stream-aligned teams to deliver work autonomously. | Delivering "self-service" capabilities (like cloud infrastructure, CI/CD pipelines, or design systems). |

---

## The 3 Interaction Modes

> To avoid chaotic, unstructured communication across teams, Team Topologies defines `three explicit ways teams interact`

### Collaboration

- Two teams work closely together for a defined, limited period to solve a shared problem or build a new capability.
  
> Use for high innovation, rapid discovery, or during early integration phases.

### X-as-a-Service

- *One team provides a service, tool, or capability that another team consumes* as a self-service product with clear APIs/documentation.

> Use for high efficiency and stability; requires minimal active communication.

### Facilitating

- One *team actively coaches or mentors another team to adopt new tools*, practices, or domain skills.

> Use when upskilling teams, clearing blockers, or adopting new industry techniques.

---

## Core Operating Principles

### Manage Cognitive Load
  
> Teams should not be overwhelmed by too many responsibilities, systems, or domains. If cognitive load is exceeded, quality and velocity drop.

### Team-First Boundaries

> Software architecture should be designed around small, long-lived, autonomous teams (typically 5 to 9 people), rather than forcing teams to adapt to rigid architectures

### The Inverse Conway Maneuver

> Design team structures deliberately to incentivize the target software architecture you want to achieve.
