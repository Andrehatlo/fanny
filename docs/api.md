# API Reference

> Document every endpoint here as it is built.
> Keep this in sync with the implementation — outdated docs cause bugs.

---

## Conventions

### Base URL

```
Development:  http://localhost:<PORT>/api
Staging:      https://staging.<domain>/api
Production:   https://<domain>/api
```

### Authentication

> Describe the auth mechanism here once chosen (JWT, session, API key, OAuth, etc.)

All protected endpoints require:

```
Authorization: Bearer <token>
```

Unauthenticated requests return `401 Unauthorized`.

### Request Format

- `Content-Type: application/json` for all POST/PUT/PATCH requests.
- Dates in ISO 8601: `2026-03-20T14:30:00Z`.
- IDs as strings (UUIDs or opaque tokens), not integers.

### Response Format

All responses follow this envelope:

```json
{
  "data": { ... },      // present on success
  "error": {            // present on failure
    "code": "VALIDATION_ERROR",
    "message": "Human-readable description",
    "details": [ ... ]  // optional field-level errors
  },
  "meta": {             // optional pagination/trace info
    "page": 1,
    "total": 42,
    "requestId": "req_abc123"
  }
}
```

### Status Codes

| Code | Meaning |
|------|---------|
| `200` | OK |
| `201` | Created |
| `204` | No content (successful delete) |
| `400` | Bad request / validation error |
| `401` | Unauthenticated |
| `403` | Forbidden (authenticated but not authorized) |
| `404` | Resource not found |
| `409` | Conflict (duplicate, stale update) |
| `422` | Unprocessable entity |
| `429` | Rate limited |
| `500` | Internal server error |

### Error Codes

| Code | HTTP | Description |
|------|------|-------------|
| `VALIDATION_ERROR` | 400 | One or more input fields failed validation |
| `UNAUTHORIZED` | 401 | Missing or invalid auth token |
| `FORBIDDEN` | 403 | Authenticated but lacks permission |
| `NOT_FOUND` | 404 | Resource does not exist |
| `CONFLICT` | 409 | Duplicate or stale resource |
| `RATE_LIMITED` | 429 | Too many requests |
| `INTERNAL_ERROR` | 500 | Unexpected server error |

---

## Endpoints

> Add endpoints as they are implemented. Use the template below.

---

### Health Check

```
GET /health
```

No authentication required.

**Response `200`:**
```json
{
  "status": "ok",
  "version": "1.0.0",
  "timestamp": "2026-03-20T14:30:00Z"
}
```

---

<!-- ENDPOINT TEMPLATE — copy for each new endpoint

### <Endpoint Name>

```
<METHOD> /api/<path>
```

**Auth required:** Yes / No

**Path parameters:**

| Param | Type | Description |
|-------|------|-------------|
| `id` | string | Resource ID |

**Query parameters:**

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `page` | integer | No | `1` | Page number |
| `limit` | integer | No | `20` | Items per page (max 100) |

**Request body:**
```json
{
  "field": "value"
}
```

**Response `200`:**
```json
{
  "data": { ... }
}
```

**Error responses:**

| Status | Code | When |
|--------|------|------|
| `400` | `VALIDATION_ERROR` | Missing required field |
| `404` | `NOT_FOUND` | Resource not found |

-->

---

*Last updated: 2026-03-20*
