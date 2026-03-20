Fix the following bug: $ARGUMENTS

Follow this process exactly:

1. **Reproduce first** — understand how to trigger the bug. Read the relevant code and confirm you understand the failure mode before touching anything.

2. **Find the root cause** — do not treat symptoms. Trace the failure to its origin.

3. **Write a failing test** — add a test in `tests/` that demonstrates the bug. Confirm it fails before fixing.

4. **Fix** — make the minimal change that resolves the root cause. Do not refactor surrounding code.

5. **Verify** — run all tests. Confirm the new test passes and no existing tests regressed.

6. **Commit** — use `fix(<scope>): <summary>` format. Include the test and the fix in the same commit, or in two sequential commits (test first).

Do not fix unrelated issues you encounter along the way. Log them as separate tasks.
