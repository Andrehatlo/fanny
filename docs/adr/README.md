# Architecture Decision Records

An ADR (Architecture Decision Record) is a short document that captures a significant architectural decision: what was decided, why, and what the trade-offs are.

---

## When to write an ADR

Write an ADR when the decision:
- Is hard to reverse (framework choice, database, auth strategy)
- Has meaningful trade-offs between alternatives
- Will affect multiple developers or components
- Involves accepting known risks or limitations

Do **not** write an ADR for implementation details, variable naming, or routine code decisions.

---

## Format

Each ADR is a Markdown file named `NNNN-short-title.md` (e.g. `0002-use-postgresql.md`).

Copy `0001-template.md` to start a new one:

```bash
cp docs/adr/0001-template.md docs/adr/000N-your-title.md
```

Increment the number by 1 for each new ADR. Never reuse a number.

---

## Status values

| Status | Meaning |
|--------|---------|
| `Proposed` | Under discussion, not yet decided |
| `Accepted` | Decision made, in effect |
| `Superseded by ADR-NNNN` | Replaced by a later decision |
| `Deprecated` | No longer relevant |

---

## Index

| # | Title | Status |
|---|-------|--------|
| [0001](0001-template.md) | Template | — |

---

*Add new ADRs to the index above when created.*
