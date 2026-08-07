
# How do you ensure shared context?

- [How do you ensure shared context?](#how-do-you-ensure-shared-context)
  - [1. Document the "Why" Not Just the "What"](#1-document-the-why-not-just-the-what)
    - [Product Requirements \& RFCs](#product-requirements--rfcs)
    - [Architecture Decision Records (ADRs)](#architecture-decision-records-adrs)
  - [2. Default to Asynchronous, Public Communication](#2-default-to-asynchronous-public-communication)
    - [Work in Public Channels](#work-in-public-channels)
    - [Write Daily/Weekly Summaries](#write-dailyweekly-summaries)
  - [3. Establish Clear Shared Artifacts](#3-establish-clear-shared-artifacts)
    - [Single Source of Truth](#single-source-of-truth)
    - [Glossary \& Terminology](#glossary--terminology)
  - [4. Run Targeted Rituals (Keep Them Focused)](#4-run-targeted-rituals-keep-them-focused)
    - [Kick-off Meetings](#kick-off-meetings)
    - [Retrospectives](#retrospectives)
  - [The Rule of Thumb](#the-rule-of-thumb)

> Ensuring shared context across a team boils down to one goal:
>> making sure everyone operates from the same mental model of
    >>>- what you are building,
    >>>- why you are building it,
    >>>- and how decisions get made.

Without deliberately building shared context, team members naturally
fill in gaps with their own assumptions—leading to

- duplicated work,
- misaligned priorities,
- and wasted energy.

Here are the most effective strategies to keep a team aligned:

---

## 1. Document the "Why" Not Just the "What"

### Product Requirements & RFCs

> Write down the problem statement and success criteria before writing code or designing.
>> When team members understand why a decision was made,
they can make autonomous trade-offs that align with the broader goal

### Architecture Decision Records (ADRs)

> Keep brief, historical logs of major technical or design choices,
> including what alternatives were rejected and why.
>> This prevents re-debating settled decisions every time a new team member joins.

---

## 2. Default to Asynchronous, Public Communication

### Work in Public Channels

> Discourage key decisions or project updates from happening in 1-on-1 direct messages
>> Moving discussions to public team channels or ticket comments allows context
 to be searchable and visible to everyone

### Write Daily/Weekly Summaries

> A quick 3-line asynchronous check-in (What got done? What’s next? What’s blocked?)
> gives the team passive visibility without taking up time with endless status meetings.

---

## 3. Establish Clear Shared Artifacts

### Single Source of Truth

> Centralize key information—whether that's a Confluence Page, or a repository `README`
>> If two places hold conflicting information, people won't know which to trust.

### Glossary & Terminology

> Agree on team-specific terms early.
> Misunderstandings often stem from simple vocabulary differences

---

## 4. Run Targeted Rituals (Keep Them Focused)

### Kick-off Meetings

> At the start of any major initiative, bring engineering, design,
> and product together to walk through goals, constraints, and non-goals.

### Retrospectives

> Regularly review what went well and where communication broke down
> so you can refine how context is shared.

---

## The Rule of Thumb

> If you find yourself explaining the same context to three different people individually
>>> it’s time to document it publicly in your team's central repository
