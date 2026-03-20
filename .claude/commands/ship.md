Prepare the current branch for merge: $ARGUMENTS

Run through this checklist and fix any issues found:

1. **Tests** — run the full test suite. All tests must pass.
2. **Lint** — run the linter. Zero warnings or errors.
3. **Format** — run the formatter. No unformatted files.
4. **Secrets scan** — confirm no secrets, tokens, or credentials are staged.
5. **Env vars** — if new env vars were added, confirm they're in `.env.example`.
6. **API docs** — if endpoints changed, confirm `docs/api.md` is updated.
7. **Commits** — ensure commit messages follow Conventional Commits format.
8. **PR description** — ensure the PR description explains what changed and why.

Report the result of each step. If anything fails, fix it before pushing.
After all checks pass, push the branch and confirm the push succeeded.
