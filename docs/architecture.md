# Architecture

> Update this document whenever the system design changes significantly.
> Keep it accurate — an outdated architecture doc is worse than none.

---

## System Overview

> Describe the system in 2–3 sentences. What does it do? Who uses it? What problem does it solve?

_(Not yet defined — fill in when the first implementation is underway.)_

---

## High-Level Diagram

```
┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   Client    │─────▶│     API     │─────▶│  Database   │
│  (browser / │      │  (server)   │      │             │
│   mobile)   │      │             │      └─────────────┘
└─────────────┘      └─────────────┘
```

> Replace with an accurate diagram when the stack is defined.
> Use ASCII art or link to an external diagram (Excalidraw, Mermaid, etc.).

---

## Components

> One section per major component. Keep each section tight.

### (Component Name)

| Field | Value |
|-------|-------|
| **Purpose** | |
| **Technology** | |
| **Entry point** | `src/...` |
| **Key dependencies** | |
| **Owned by** | |

---

## Data Flow

> Describe how data moves through the system for the primary use cases.
> Use numbered steps or a sequence diagram.

```
1. User submits form
2. Client sends POST /api/...
3. API validates input
4. API writes to database
5. API returns response
6. Client updates UI
```

---

## Key Design Decisions

> Brief summary — full details live in `docs/adr/`.

| Decision | Choice | ADR |
|----------|--------|-----|
| _(none yet)_ | | |

---

## Boundaries & Interfaces

> What does this system integrate with externally?

| External system | Direction | Protocol | Notes |
|----------------|-----------|----------|-------|
| _(none yet)_ | | | |

---

## Scalability & Performance

> Document known limits, bottlenecks, and strategies.

_(Not yet defined.)_

---

## Known Limitations & Tech Debt

> Be honest. Document shortcuts, known issues, and deferred work.

_(None logged yet.)_

---

*Last updated: 2026-03-20*
