---
name: modern-frontend-workflow
description: Use when working on modern frontend projects with React, Next.js, Tailwind CSS, or similar stacks. Triggers on frontend architecture, component design, state management, styling systems, build optimization, and best practices for production-ready UI.
---

# Modern Frontend Workflow

## Core Principles

- Prefer composition over inheritance.
- Keep components small and focused on a single responsibility.
- Colocate related files (component + styles + tests + types).
- Avoid premature abstraction. Extract only when a pattern repeats 3+ times.
- Favor server components and progressive enhancement when using Next.js or similar frameworks.

## Recommended Stack Defaults

- Framework: Next.js (App Router) or Vite + React
- Styling: Tailwind CSS + shadcn/ui or similar accessible component library
- State: Server state via React Query / TanStack Query. Local UI state with useState/useReducer. Avoid global client state unless truly necessary.
- Forms: React Hook Form + Zod
- Icons: Lucide React
- Animation: Framer Motion (sparingly)

## Project Structure (App Router example)

```
src/
├── app/                    # routes and layouts
├── components/
│   ├── ui/                 # base design system components
│   └── features/           # feature-specific components
├── lib/                    # utilities, helpers, constants
├── hooks/                  # custom hooks
├── types/                  # shared TypeScript types
└── styles/                 # global styles if needed
```

## Component Guidelines

- Use TypeScript for all components.
- Prefer named exports for components.
- Accept `className` prop and merge with `cn()` utility (clsx + tailwind-merge).
- Keep business logic out of presentational components.
- Handle loading, empty, and error states explicitly.

## Performance Checklist

- Use `next/image` or equivalent optimized image component.
- Lazy load non-critical components with dynamic imports.
- Avoid large client-side bundles. Push logic to the server when possible.
- Measure Core Web Vitals before shipping major UI changes.

## When Reviewing or Generating Code

1. Check for unnecessary client components (`"use client"`).
2. Ensure accessibility (keyboard navigation, ARIA where needed, semantic HTML).
3. Verify responsive design works on mobile first.
4. Confirm consistent spacing, typography, and color usage from the design system.
