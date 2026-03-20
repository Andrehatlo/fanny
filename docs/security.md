# Security

---

## Reporting Vulnerabilities

**Do not open a public GitHub issue for security vulnerabilities.**

Report privately:
- Email: _(add security contact email)_
- GitHub private vulnerability reporting: `Security` tab → `Report a vulnerability`

Include a description of the vulnerability, steps to reproduce, and potential impact.
You will receive a response within 48 hours.

---

## Threat Model

> Fill in as the application is built. Document what we protect, who the adversaries are, and what the acceptable risk is.

| Asset | Threat | Mitigation |
|-------|--------|-----------|
| _(none yet)_ | | |

---

## Security Controls

### Authentication & Authorization

- _(To be defined — e.g. JWT with short expiry + refresh tokens)_
- All protected routes verify the token on every request.
- Authorization is checked at the handler level, not just middleware.

### Input Validation

- Validate all user input at the API boundary before processing.
- Reject requests with unexpected or malformed fields.
- Use an allowlist approach: define what is valid, reject everything else.

### Database

- Use parameterized queries / prepared statements. **Never** concatenate user input into SQL.
- Database user has minimum necessary permissions (no `DROP`, no `CREATE` from app).
- Sensitive fields (passwords) are hashed with bcrypt (cost ≥ 12) or Argon2id. Never stored in plaintext.

### Secrets Management

- Secrets live in environment variables, never in source code.
- `.env` is in `.gitignore`. `.env.example` contains no real values.
- Rotate secrets immediately if they are ever exposed.
- In CI/CD, use GitHub Actions Secrets (`Settings → Secrets and variables`).

### HTTP Security Headers

When serving HTTP responses, include:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=(), camera=()
```

### Dependencies

- Pin dependency versions in the lockfile.
- Run `npm audit` / `pip-audit` / `cargo audit` in CI.
- Review changelogs before upgrading major versions.
- Remove unused dependencies.

### Logging

- **Never log** passwords, tokens, cookies, or PII.
- Log authentication events (login, logout, failed attempts) with IP and timestamp.
- Log authorization failures.
- Use structured logging (JSON) with a `severity` field.

### Rate Limiting

- Apply rate limiting to all public endpoints.
- Apply stricter limits to auth endpoints (login, password reset).
- Return `429 Too Many Requests` with a `Retry-After` header.

---

## Security Checklist for New Features

Before shipping any new feature, verify:

- [ ] All inputs validated and sanitized
- [ ] Authorization checked (not just authentication)
- [ ] No secrets in code or logs
- [ ] SQL uses parameterized queries
- [ ] Error messages don't leak internals to clients
- [ ] Sensitive operations are logged (with no sensitive data in the log)
- [ ] Rate limiting applied if the endpoint is public or abusable

---

*Last updated: 2026-03-20*
