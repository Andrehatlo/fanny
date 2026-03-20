# CLAUDE.md

> **AI assistants: read this file in full before making any changes.**
> It is the single source of truth for how this repository works.

---

## 1. Repository Identity

| Field | Value |
|-------|-------|
| **Repo** | `Andrehatlo/fanny` |
| **Status** | Foundation established — no application code yet |
| **Language/stack** | To be determined by first implementation request |
| **Last updated** | 2026-03-20 |

When the stack is chosen, update the table above and fill in the placeholder sections below.

---

## 2. Absolute Rules (never break these)

1. **Never commit to `main` directly.** All changes go through a branch + PR.
2. **Never commit secrets.** No `.env`, API keys, passwords, tokens, or credentials. Use `.env.example` for templates.
3. **Never skip tests.** If tests exist, run them before committing. Do not commit code that breaks tests.
4. **Never force-push without confirmation.** Ask the user before any destructive git operation.
5. **Never introduce a dependency without a clear reason.** Prefer stdlib/built-ins. Every new dep must solve a real problem.
6. **Never over-engineer.** Solve the stated problem, no more. Do not add speculative features or abstractions.
7. **Never silently swallow errors.** All errors must be logged or surfaced. No empty catch blocks.

---

## 3. Project Structure

```
fanny/
├── .claude/
│   ├── commands/             # Custom Claude Code slash commands
│   └── settings.json         # Claude Code tool permissions & hooks
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── workflows/
│       └── ci.yml
├── docs/
│   ├── adr/                  # Architecture Decision Records
│   │   ├── README.md
│   │   └── 0001-template.md
│   ├── architecture.md       # System design & component map
│   ├── api.md                # API conventions & endpoint reference
│   ├── contributing.md       # How to contribute
│   └── security.md           # Security policies & threat model
├── scripts/                  # Dev/ops utility scripts
├── src/                      # Application source code
├── tests/                    # Test suites
├── .env.example              # Required environment variables (no values)
├── .gitignore
├── CLAUDE.md                 # This file
└── README.md
```

**Rules:**
- Source code lives in `src/`. No logic in root-level files.
- Tests mirror the `src/` structure inside `tests/`.
- One-off utility scripts go in `scripts/`, not in `src/`.
- Documentation goes in `docs/`. Do not scatter `.md` files across `src/`.

---

## 4. Stack & Tooling

> Fill this section in when the stack is chosen. Delete the placeholder text.

| Concern | Choice | Notes |
|---------|--------|-------|
| Language | TBD | |
| Runtime | TBD | |
| Framework | TBD | |
| Database | TBD | |
| ORM / query builder | TBD | |
| Auth | TBD | |
| Testing | TBD | |
| Linter | TBD | |
| Formatter | TBD | |
| CI | GitHub Actions | See `.github/workflows/ci.yml` |
| Package manager | TBD | |

---

## 5. Getting Started

```bash
# Clone
git clone https://github.com/Andrehatlo/fanny.git
cd fanny

# Copy environment variables
cp .env.example .env
# Fill in real values in .env (never commit this file)

# Install dependencies (update command once stack is chosen)
# npm install | pip install -r requirements.txt | cargo build | etc.

# Run development server
# npm run dev | python main.py | cargo run | etc.

# Run tests
# npm test | pytest | cargo test | etc.
```

---

## 6. Development Workflow

### 6.1 Branch Strategy

```
main           ← protected, always deployable
  └─ feature/<ticket-or-description>    ← new functionality
  └─ fix/<ticket-or-description>        ← bug fixes
  └─ chore/<description>                ← deps, config, tooling
  └─ docs/<description>                 ← documentation only
  └─ claude/<description>-<session-id>  ← AI-generated branches
```

- Branch from `main`.
- Merge back to `main` via PR with at least 1 approval.
- Delete branches after merge.

### 6.2 Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <imperative summary under 72 chars>

[optional body — explain WHY, not what]

[optional footer — breaking changes, issue refs]
```

| Type | When to use |
|------|-------------|
| `feat` | New feature visible to users |
| `fix` | Bug fix |
| `refactor` | Code change with no behavior change |
| `test` | Adding or updating tests |
| `docs` | Documentation only |
| `chore` | Deps, build, tooling, CI |
| `style` | Formatting only (no logic change) |
| `perf` | Performance improvement |

**Good examples:**
```
feat(auth): add OAuth2 login with GitHub provider
fix(api): return 404 instead of 500 for missing resources
test(user): add integration tests for password reset flow
chore: upgrade dependencies to latest patch versions
```

**Bad examples:**
```
fix stuff           ← too vague
WIP                 ← never commit WIP to a shared branch
Updated files       ← meaningless
```

### 6.3 Pull Requests

- Use the PR template in `.github/PULL_REQUEST_TEMPLATE.md`.
- Keep PRs focused — one concern per PR.
- All CI checks must pass before merge.
- Link issues with `Closes #<n>` or `Fixes #<n>`.
- Squash trivial fixup commits before requesting review.

---

## 7. Code Conventions

> These are universal rules. Stack-specific rules go in `docs/contributing.md`.

### Naming

| Thing | Convention | Example |
|-------|-----------|---------|
| Files | `kebab-case` | `user-service.ts` |
| Folders | `kebab-case` | `auth-middleware/` |
| Variables/functions | `camelCase` (JS/TS) / `snake_case` (Python/Go) | |
| Constants | `SCREAMING_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| Classes/Types | `PascalCase` | `UserRepository` |
| Boolean vars | `is`, `has`, `should`, `can` prefix | `isLoading`, `hasError` |

### Functions

- Single responsibility — one function does one thing.
- Maximum 30 lines. If longer, extract helpers.
- Name functions after what they **do** (verb + noun): `fetchUser`, `validateEmail`.
- No side effects in functions named as queries (`get*`, `find*`, `list*`).

### Error Handling

- Never swallow errors silently.
- Fail loudly in development, fail gracefully in production.
- Log the original error + context, not just a generic message.
- Distinguish user errors (4xx) from system errors (5xx) in APIs.

### Comments

- Comment **why**, never **what**. Code explains what; comments explain intent.
- Remove commented-out code — use git history instead.
- TODOs must include an owner: `// TODO(andrehatlo): fix after API v2 migration`

### Security (always)

- Validate all external input at system boundaries.
- Never trust user-supplied data in SQL, shell commands, or HTML.
- Never log sensitive data (passwords, tokens, PII).
- Use parameterized queries — never string-concatenated SQL.
- See `docs/security.md` for the full threat model.

---

## 8. Testing

### Philosophy

- **Test behavior, not implementation.** Tests should survive refactors.
- **Arrange → Act → Assert** structure in every test.
- Every bug fix ships with a regression test.
- Aim for high coverage on business logic; don't chase 100% on trivial code.

### Test Types

| Type | Location | Runs |
|------|----------|------|
| Unit | `tests/unit/` | On every commit |
| Integration | `tests/integration/` | On every PR |
| End-to-end | `tests/e2e/` | On merge to main |

### Commands (update once stack is chosen)

```bash
# Run all tests
# npm test | pytest | cargo test

# Run specific file
# npm test -- path/to/test | pytest tests/unit/foo_test.py

# Watch mode
# npm test -- --watch | pytest-watch

# Coverage report
# npm test -- --coverage | pytest --cov=src
```

---

## 9. Environment Variables

All required variables must have an entry in `.env.example` with a description comment.

```bash
# Example format in .env.example:
# DATABASE_URL=          # PostgreSQL connection string
# JWT_SECRET=            # Min 32 chars, randomly generated
# PORT=3000              # HTTP server port
```

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| _(none yet)_ | | | |

**Rules:**
- `.env` is in `.gitignore` — never commit it.
- `.env.example` is committed — it contains keys but **no real values**.
- In CI, use GitHub Actions secrets (`Settings → Secrets`).
- Never access `process.env` (or equivalent) outside of a single config module.

---

## 10. CI/CD

Pipeline defined in `.github/workflows/ci.yml`.

| Trigger | Jobs |
|---------|------|
| Every push | lint, test |
| PR to main | lint, test, build |
| Merge to main | lint, test, build, deploy (when configured) |

All jobs must pass before a PR can be merged.

---

## 11. Architecture Decision Records (ADRs)

Significant decisions live in `docs/adr/`. When making a non-trivial architectural choice (framework selection, database choice, auth strategy, major refactor), create an ADR.

```bash
# Create a new ADR
cp docs/adr/0001-template.md docs/adr/000N-short-title.md
# Fill in context, decision, and consequences
```

See `docs/adr/README.md` for format guidance.

---

## 12. AI Assistant Playbook

### Before starting any task

1. Read `CLAUDE.md` (this file) in full.
2. Read `docs/architecture.md` if it exists.
3. Search for existing patterns before introducing new ones.
4. Read the files you will modify before touching them.

### While working

- Match the style of surrounding code exactly.
- Make the smallest change that satisfies the requirement.
- Don't refactor code you weren't asked to change.
- Don't add comments, types, or error handling to code you didn't write.
- Run the linter and tests before committing.

### Commits & branches

- Develop on `claude/<description>-<session-id>` branches.
- Push to that branch only — never to `main`.
- Write commit messages that follow section 6.2.
- One logical change per commit.

### When unsure

- Ask before doing destructive operations.
- Ask before choosing a technology that isn't already in the stack.
- Ask before creating new top-level directories.
- Do not guess environment variable values — ask.

### What NOT to do

- Do not add dependencies without asking.
- Do not rename files or reorganize folders unless explicitly asked.
- Do not change unrelated code while fixing a bug.
- Do not add logging, metrics, or tracing unless asked.
- Do not generate mock data, seed scripts, or example files unless asked.
- Do not create README files for every folder.

---

## 13. Glossary

| Term | Definition |
|------|-----------|
| ADR | Architecture Decision Record — a short document explaining a significant design choice |
| Conventional Commits | A commit message format: `type(scope): summary` |
| Trunk | The `main` branch — always deployable |

---

*Last updated: 2026-03-20*
