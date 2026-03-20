Implement the following new feature: $ARGUMENTS

Follow this process exactly:

1. **Understand first** — read `CLAUDE.md`, then `docs/architecture.md`. Search the codebase for related existing patterns before writing any code.

2. **Plan** — briefly describe (in a code comment or to the user) what files you will touch and why, before making changes.

3. **Implement** — write the minimal code that satisfies the requirement. Match the style of surrounding code exactly.

4. **Test** — write or update tests in `tests/` that cover the new behavior. Run all tests and confirm they pass.

5. **Document** — if the feature adds or changes an API endpoint, update `docs/api.md`. If a significant architecture decision was made, create an ADR in `docs/adr/`.

6. **Commit** — use Conventional Commits format: `feat(<scope>): <summary>`. One commit per logical change.

Do not add extra dependencies, abstractions, or features beyond what was asked.
