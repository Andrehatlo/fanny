# CLAUDE.md

This file provides guidance for AI assistants (Claude and others) working in this repository.

## Repository Overview

**Repo:** `Andrehatlo/fanny`
**Status:** Newly initialized — no source code has been added yet.

This repository is a blank slate. Update this file as the project evolves to keep AI assistants accurately informed about architecture, conventions, and workflows.

---

## Getting Started (to be filled in)

Once the project is set up, document the following here:

```bash
# Install dependencies
# e.g. npm install / pip install -r requirements.txt / cargo build

# Run development server
# e.g. npm run dev

# Run tests
# e.g. npm test

# Build for production
# e.g. npm run build
```

---

## Repository Structure (to be filled in)

Document the intended directory layout here as the project develops. Example:

```
fanny/
├── src/              # Application source code
│   ├── components/   # UI components (if frontend)
│   ├── lib/          # Shared utilities
│   └── ...
├── tests/            # Test files
├── docs/             # Documentation
├── .github/
│   └── workflows/    # CI/CD pipelines
├── package.json      # (or equivalent manifest)
└── CLAUDE.md         # This file
```

---

## Development Workflow

### Branch Strategy

- **Main branch:** `main` (or `master`) — protected; never commit directly
- **Feature branches:** `feature/<short-description>`
- **Bug fix branches:** `fix/<short-description>`
- **Claude-created branches:** `claude/<description>-<session-id>` (auto-named by Claude Code)

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<optional scope>): <short summary>

[optional body]

[optional footer]
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Examples:
```
feat(auth): add JWT refresh token support
fix(api): handle null response from upstream service
docs: update CLAUDE.md with project structure
```

### Pull Requests

1. Keep PRs focused and small
2. Write a clear PR description summarizing the change and why
3. Ensure all CI checks pass before requesting review
4. Link related issues using `Closes #<issue-number>` in the PR body

---

## Code Conventions (to be filled in)

Document language/framework-specific conventions here as they are established. Examples of what to include:

- **Language & runtime version** (e.g. Node 20, Python 3.12, Rust 1.77)
- **Formatter & linter** (e.g. Prettier + ESLint, Black + Ruff, rustfmt + Clippy)
- **Testing framework** (e.g. Jest, pytest, cargo test)
- **Code style rules** (e.g. 2-space indent, double quotes, no semicolons)
- **Import ordering** conventions
- **Naming conventions** (files, variables, functions, types)

---

## Testing

Once tests are configured, document:

```bash
# Run all tests
# Run a single test file
# Run tests in watch mode
# Generate coverage report
```

---

## Environment Variables

Document required environment variables here. Use a `.env.example` file in the repo for local development.

| Variable | Description | Required |
|----------|-------------|----------|
| _(none yet)_ | | |

Never commit secrets or `.env` files to the repository.

---

## CI/CD

Document the CI/CD pipeline here once GitHub Actions (or another system) is configured. Include:

- What triggers CI (push, PR, schedule)
- What checks run (lint, test, build, deploy)
- Deployment targets and environments

---

## Key Decisions & Architecture Notes

Use this section to record significant architectural decisions (ADRs) as they are made:

- _(none yet — add decisions here as the project grows)_

---

## Working with AI Assistants

Guidelines for Claude and other AI assistants working in this repo:

1. **Read this file first** before making changes to understand current conventions.
2. **Check for existing patterns** before introducing new ones — search the codebase.
3. **Keep changes focused** — match the scope of what was requested.
4. **Run tests before committing** — do not commit code that breaks existing tests.
5. **Update CLAUDE.md** when significant architectural decisions or conventions are established.
6. **Never commit secrets**, credentials, or `.env` files.
7. **Ask before destructive operations** — force pushes, branch deletions, database drops.
8. **Branch naming:** develop on `claude/<description>-<session-id>` branches; push to those branches only.

---

*Last updated: 2026-03-20 — initial creation (empty repository)*
