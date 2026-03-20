Review the following code or PR: $ARGUMENTS

Evaluate and report on:

1. **Correctness** — does the code do what it claims? Are there edge cases, off-by-one errors, null dereferences, or race conditions?

2. **Security** — does it validate inputs? Are there injection risks (SQL, command, XSS)? Are secrets handled safely? See `docs/security.md`.

3. **Tests** — are the right behaviors tested? Do tests test behavior or implementation details? Are edge cases covered?

4. **Conventions** — does the code follow `CLAUDE.md` section 7 (naming, function size, error handling, comments)?

5. **Simplicity** — is this the simplest implementation? Is there unnecessary complexity, premature abstraction, or dead code?

6. **Documentation** — if the change affects the API, is `docs/api.md` updated? If it's a significant architectural change, is there an ADR?

Format your review as:

**Blocking issues** (must fix before merge):
- ...

**Suggestions** (non-blocking improvements):
- ...

**Looks good** (things done well):
- ...
