---
name: backend-api-design
description: Use when designing, reviewing, or implementing REST or GraphQL APIs. Triggers on API endpoints, request response structure, authentication, error handling, versioning, documentation, and backend architecture decisions.
---

# Backend API Design

## Core Principles

- Design APIs around resources and use cases, not database tables.
- Prefer explicit over clever. Make the happy path obvious.
- Consistency beats perfection. Establish conventions and stick to them.
- Always design for failure. Every endpoint must handle errors gracefully.

## REST API Conventions

- Use nouns for resources: `/users`, `/projects/{id}/tasks`
- HTTP methods:
  - GET → read
  - POST → create
  - PUT/PATCH → update
  - DELETE → remove
- Use plural nouns for collections.
- Nest resources only one level deep when possible.
- Version via URL prefix (`/api/v1/`) or header when needed.

## Response Structure

Standard success response:
```json
{
  "data": {},
  "meta": {
    "page": 1,
    "total": 100
  }
}
```

Standard error response:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Human readable message",
    "details": []
  }
}
```

## Essential Practices

- Validate all input at the boundary (Zod, Joi, or equivalent).
- Never trust client-sent data.
- Use proper HTTP status codes (200, 201, 400, 401, 403, 404, 422, 500).
- Implement rate limiting on public endpoints.
- Log request ID for every request to enable tracing.
- Document with OpenAPI / Swagger and keep it up to date.

## Authentication & Authorization

- Prefer JWT or session-based auth with short-lived tokens + refresh tokens.
- Separate authentication (who you are) from authorization (what you can do).
- Always check permissions on the server side.
- Never expose internal IDs or sensitive fields in responses.

## When Generating or Reviewing API Code

1. Check for consistent naming and response shape.
2. Ensure proper error handling and status codes.
3. Verify input validation exists.
4. Confirm sensitive data is not leaked.
5. Look for missing pagination on list endpoints.
