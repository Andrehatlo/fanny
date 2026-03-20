# Contributing Guide

---

## Prerequisites

> Update this list once the stack is defined.

- Git ≥ 2.40
- _(Language runtime — e.g. Node ≥ 20, Python ≥ 3.12)_
- _(Package manager — e.g. npm, pip, cargo)_

---

## Local Setup

```bash
git clone https://github.com/Andrehatlo/fanny.git
cd fanny
cp .env.example .env        # fill in values
# install deps + start server (fill in commands once stack is chosen)
```

---

## Making Changes

1. Pull the latest `main`:
   ```bash
   git checkout main && git pull origin main
   ```

2. Create a branch:
   ```bash
   git checkout -b feature/my-change
   ```

3. Make changes. Follow the conventions in `CLAUDE.md` sections 7 and 8.

4. Run linter and tests:
   ```bash
   # fill in once stack is chosen
   ```

5. Commit using [Conventional Commits](https://www.conventionalcommits.org/):
   ```bash
   git commit -m "feat(scope): what and why"
   ```

6. Push and open a PR:
   ```bash
   git push -u origin feature/my-change
   # Open PR on GitHub, use the PR template
   ```

---

## Code Style

- Run the formatter before committing. CI will fail if formatting is off.
- Follow naming conventions in `CLAUDE.md` section 7.
- Do not mix formatting changes with logic changes in the same commit.

---

## Testing

- Write tests for every new feature and every bug fix.
- Tests live in `tests/`, mirroring the structure of `src/`.
- All tests must pass locally before opening a PR.

---

## PR Review Checklist

Before requesting review, verify:

- [ ] Tests pass locally
- [ ] Linter passes with no warnings
- [ ] New code has corresponding tests
- [ ] `.env.example` updated if new env vars added
- [ ] `docs/api.md` updated if endpoints added/changed
- [ ] `docs/architecture.md` updated if design changed
- [ ] An ADR created in `docs/adr/` if a significant decision was made
- [ ] PR description explains **why**, not just what

---

## Reviewing PRs

- Be specific: point to the line, explain the concern.
- Distinguish blocking issues from suggestions: prefix with `nit:` for non-blocking.
- Approve only when you are confident the change is correct and safe.

---

## Reporting Bugs

Use the GitHub issue template: `.github/ISSUE_TEMPLATE/bug_report.md`.

Include:
- Steps to reproduce (exact, minimal)
- Expected behavior
- Actual behavior
- Environment (OS, runtime version, browser if relevant)
- Relevant logs or screenshots

---

## Suggesting Features

Use the GitHub issue template: `.github/ISSUE_TEMPLATE/feature_request.md`.

Include:
- The problem you're trying to solve
- Proposed solution
- Alternatives considered

---

*Last updated: 2026-03-20*
