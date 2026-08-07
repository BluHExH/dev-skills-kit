---
name: fullstack-project-structure
description: Use when starting a new full-stack project or reorganizing an existing codebase. Triggers on folder structure, monorepo setup, project scaffolding, architecture decisions, and how to organize frontend + backend + shared code.
---

# Full-stack Project Structure

## Recommended Structure (Monorepo style)

```
project-root/
├── apps/
│   ├── web/                 # Frontend (Next.js / Vite)
│   └── api/                 # Backend (Node, Nest, Fastify, etc.)
├── packages/
│   ├── ui/                  # Shared UI components
│   ├── config/              # Shared ESLint, TSConfig, Tailwind
│   ├── database/            # Prisma / Drizzle schema + client
│   └── shared/              # Shared types, utils, constants
├── package.json             # Root workspace
└── turbo.json / pnpm-workspace.yaml
```

## Alternative (Simple Single Repo)

```
src/
├── app/ or pages/           # Frontend routes
├── components/
├── server/ or api/          # Backend routes / controllers
├── lib/
│   ├── db.ts
│   ├── auth.ts
│   └── utils.ts
├── types/
└── styles/
```

## Key Rules

- Keep frontend and backend concerns separated even in the same repo.
- Put shared types in one place so both sides stay in sync.
- Configuration files (ESLint, Prettier, TypeScript) should live at the root or in a shared package.
- Environment variables must be clearly documented and never committed.
- Database schema and migrations belong in their own package or folder.

## Naming Conventions

- Use kebab-case for folders and files in most cases.
- Use PascalCase for React components.
- Prefer index.ts only when it improves import ergonomics, not by default.

## When Setting Up a New Project

1. Decide monorepo vs single package based on team size and complexity.
2. Set up TypeScript strictly from day one.
3. Configure linting + formatting + pre-commit hooks early.
4. Create a clear README with setup instructions and environment variables.
5. Establish a consistent import alias strategy (`@/`, `@shared/` etc.).
