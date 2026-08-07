---
name: backend-api-design
description: Use when designing, reviewing, implementing, or documenting REST or GraphQL APIs. Triggers on endpoint design, request/response structure, authentication, authorization, error handling, versioning, pagination, rate limiting, and production API best practices.
---

# Backend API Design

Production-grade guidelines for designing clean, consistent, and maintainable APIs in 2026.

## Core Principles

1. **Design for the consumer** — Optimize for the developer who will call your API, not for your database schema.
2. **Consistency beats perfection** — Establish clear conventions and follow them everywhere.
3. **Fail explicitly** — Every error should be predictable and well-documented.
4. **Security by default** — Never trust client input. Validate everything at the boundary.
5. **Evolve carefully** — Breaking changes are expensive. Version and deprecate intentionally.

## REST API Conventions

### Resource Naming
- Use **nouns**, not verbs: `/users`, `/projects`, `/orders`
- Use **plural** names for collections
- Keep nesting shallow (max 1-2 levels): `/projects/{id}/tasks` is acceptable, deeper is usually a smell
- Avoid actions in URLs. Prefer HTTP methods + resource state

### HTTP Methods
| Method | Usage | Idempotent |
|--------|-------|------------|
| GET | Read | Yes |
| POST | Create | No |
| PUT | Full replace | Yes |
| PATCH | Partial update | No (usually) |
| DELETE | Remove | Yes |

### Status Codes (Use Correctly)
- `200` OK — Successful GET, PUT, PATCH
- `201` Created — Successful POST that creates a resource
- `204` No Content — Successful DELETE or action with no body
- `400` Bad Request — Validation failed / malformed request
- `401` Unauthorized — Missing or invalid authentication
- `403` Forbidden — Authenticated but not allowed
- `404` Not Found — Resource does not exist
- `409` Conflict — State conflict (e.g. duplicate)
- `422` Unprocessable Entity — Semantically invalid
- `429` Too Many Requests — Rate limited
- `500` Internal Server Error — Unexpected server failure

## Response Structure

**Success (single resource):**
```json
{
  "data": {
    "id": "usr_123",
    "email": "user@example.com",
    "createdAt": "2026-01-15T10:30:00Z"
  }
}
```

**Success (collection):**
```json
{
  "data": [...],
  "meta": {
    "page": 1,
    "perPage": 20,
    "total": 150,
    "totalPages": 8
  }
}
```

**Error:**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is required",
    "details": [
      { "field": "email", "message": "Required" }
    ]
  }
}
```

Keep the shape consistent across the entire API.

## Input Validation

- Validate **all** incoming data at the API boundary.
- Prefer schema-based validation (Zod, Joi, Yup, class-validator, etc.).
- Return clear field-level errors.
- Never trust query params, headers, or body content.
- Sanitize or reject unexpected fields if necessary.

## Authentication & Authorization

- Prefer short-lived access tokens + refresh tokens.
- Common solid options in 2026: Auth.js, Clerk, Supabase Auth, Better Auth, or custom JWT with proper rotation.
- Always verify permissions **on the server**. Never rely only on frontend checks.
- Use clear separation:
  - Authentication = Who are you?
  - Authorization = What are you allowed to do?
- Prefer resource-level and role/permission based checks over hard-coded user IDs.

## Pagination

Always paginate list endpoints.

**Cursor-based (preferred for large/dynamic data):**
```
GET /items?cursor=abc123&limit=20
```

**Offset-based (acceptable for smaller/admin data):**
```
GET /items?page=2&perPage=20
```

Return clear pagination metadata so clients can build proper UI.

## Versioning

Choose one strategy and stick to it:
- URL versioning: `/api/v1/...` (most common and explicit)
- Header versioning (less visible)

Never break existing clients without a deprecation period. Document breaking changes clearly.

## Rate Limiting & Abuse Protection

- Apply rate limits on public and sensitive endpoints.
- Return `429` with a clear `Retry-After` header when possible.
- Consider different limits for authenticated vs anonymous users.
- Protect expensive operations (search, exports, AI calls, etc.).

## Documentation

- Keep OpenAPI / Swagger (or equivalent) up to date.
- Document:
  - Authentication method
  - Every endpoint with request/response examples
  - Error codes and meanings
  - Rate limits
  - Pagination style
- Good docs reduce support load dramatically.

## Error Handling Guidelines

- Never leak stack traces or internal details in production responses.
- Log full error context server-side (with request ID).
- Return stable error codes that clients can rely on.
- Make error messages helpful for developers, not just generic.

## Security Checklist

- [ ] All inputs validated
- [ ] Authentication required where needed
- [ ] Authorization checked on every sensitive operation
- [ ] HTTPS only
- [ ] Sensitive data never logged
- [ ] Rate limiting in place
- [ ] CORS configured correctly
- [ ] No sensitive data in URLs
- [ ] Proper CORS and security headers

## Common Mistakes to Avoid

1. Designing endpoints around database tables instead of use cases.
2. Inconsistent response shapes across endpoints.
3. Using 200 for everything (including errors).
4. Returning different error formats in different places.
5. Missing pagination on list endpoints.
6. Putting verbs in URLs (`/getUser`, `/createOrder`).
7. Trusting client-side validation only.
8. Exposing internal IDs or implementation details.

## When Designing or Reviewing an API

1. Can a new developer understand the endpoint from the URL + method alone?
2. Is the response shape consistent with the rest of the API?
3. Are errors clear and actionable?
4. Is pagination present on collections?
5. Are authentication and authorization correctly enforced?
6. Is there unnecessary nesting or complexity?
7. Will this design still work when the product grows?

## Production Readiness Checklist

- [ ] Consistent naming and response format
- [ ] Proper HTTP status codes
- [ ] Input validation on all endpoints
- [ ] Authentication & authorization implemented
- [ ] Pagination on list endpoints
- [ ] Rate limiting on public endpoints
- [ ] Clear error format
- [ ] OpenAPI/Swagger documentation
- [ ] Request ID / correlation ID for tracing
- [ ] Logging and monitoring in place
