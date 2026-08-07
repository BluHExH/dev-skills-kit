---
name: modern-frontend-workflow
description: Use when building or reviewing modern frontend applications with React, Next.js, TypeScript, and Tailwind CSS. Triggers on component architecture, App Router patterns, state management decisions, styling systems, performance, accessibility, and production-ready frontend workflows.
---

# Modern Frontend Workflow

Production-grade guidance for building high-quality frontend applications in 2026. Focused on Next.js App Router + React + TypeScript + Tailwind as the default stack.

## Core Philosophy

1. **Server-first by default** — Prefer Server Components. Only add `"use client"` when you need interactivity, browser APIs, or hooks.
2. **Composition over configuration** — Small, focused components that compose well beat large configurable ones.
3. **Colocation** — Keep related files close (component + styles + tests + types).
4. **Explicit over clever** — Readable code wins long-term.
5. **Performance is a feature** — Measure and protect Core Web Vitals from day one.

## Recommended Stack (2026)

| Layer | Recommendation | Notes |
|-------|----------------|-------|
| Framework | Next.js (App Router) | Default choice for most products |
| Language | TypeScript (strict) | Non-negotiable |
| Styling | Tailwind CSS + CSS Variables | + shadcn/ui or similar for primitives |
| Components | shadcn/ui / Radix based | Copy-paste, full ownership |
| Server State | TanStack Query | For any client-fetched data |
| Forms | React Hook Form + Zod | Best DX + type safety |
| Animation | Framer Motion or CSS | Use sparingly |
| Icons | Lucide React | Consistent and tree-shakeable |
| Validation | Zod | Shared between client and server |

## Project Structure

```
src/
├── app/                          # App Router
│   ├── (marketing)/              # Route groups
│   ├── (app)/
│   ├── api/
│   ├── layout.tsx
│   └── globals.css
├── components/
│   ├── ui/                       # Design system primitives (shadcn)
│   ├── features/                 # Feature-specific components
│   └── layouts/
├── hooks/                        # Custom React hooks
├── lib/                          # Utilities, cn(), api clients
├── types/
└── styles/
```

### Rules for structure
- Never put business logic inside `ui/` components.
- Feature folders can contain their own components, hooks, and utils.
- Prefer named exports for components.
- Use absolute imports via `@/`.

## Server vs Client Components

**Default to Server Components.**

Add `"use client"` only when the component needs:
- useState, useEffect, useRef, or other hooks
- Browser-only APIs (window, document, localStorage)
- Event handlers that cannot be passed as props from server
- Third-party libraries that require the client

**Pattern:**
```tsx
// Server Component (default)
async function Page() {
  const data = await getData()
  return <ClientInteractive data={data} />
}

// Client Component
"use client"
function ClientInteractive({ data }) {
  const [state, setState] = useState()
  // ...
}
```

Push interactivity as far down the tree as possible.

## Component Guidelines

- One component = one responsibility.
- Accept `className` and merge with `cn()` (clsx + tailwind-merge).
- Prefer composition (`children`, slots) over props explosion.
- Handle loading, empty, and error states explicitly.
- Keep prop interfaces small. Extract complex objects when needed.
- Use TypeScript interfaces/types for all public props.

## Styling Rules

- Use Tailwind utility classes as the primary styling method.
- Create design tokens via CSS variables in `globals.css`.
- Avoid arbitrary values (`w-[123px]`) unless truly one-off.
- Use `cn()` for conditional classes.
- Prefer shadcn/ui primitives over custom one-off components when possible.
- Dark mode should be class-based (`class="dark"`) and flash-free.

## State Management Decision Tree

1. **URL state** (filters, tabs, pagination) → Prefer searchParams
2. **Server data** → TanStack Query / Server Components
3. **Local UI state** (modals, toggles) → useState / useReducer
4. **Global client state** (rarely needed) → Zustand
5. Avoid Redux/Context for simple cases.

## Forms

Standard pattern:

```tsx
"use client"
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { z } from "zod"

const schema = z.object({
  email: z.string().email(),
})

type FormValues = z.infer<typeof schema>

function ExampleForm() {
  const form = useForm<FormValues>({
    resolver: zodResolver(schema),
  })

  // ...
}
```

Always validate on both client and server.

## Performance Checklist

- Use `next/image` for all images with proper sizes and priority.
- Dynamic import heavy components (`next/dynamic`).
- Avoid large client bundles. Audit with `@next/bundle-analyzer`.
- Prefer Server Components for data fetching.
- Cap `dpr` and be careful with third-party scripts.
- Measure LCP, INP, CLS regularly.
- Lazy load below-the-fold content.

## Accessibility Baseline

- Semantic HTML first.
- Keyboard navigation must work.
- Proper focus management for modals and dialogs.
- Color contrast meets WCAG AA.
- Provide labels for form controls.
- Respect `prefers-reduced-motion`.

## Common Mistakes to Avoid

1. Marking entire pages as `"use client"` unnecessarily.
2. Fetching data in Client Components when it could be done on the server.
3. Prop drilling instead of composition.
4. Creating new objects/functions inside render without memoization when it matters.
5. Ignoring loading and error states.
6. Using `any` or disabling TypeScript checks.
7. Over-abstracting too early (Rule of Three).

## When Generating or Reviewing Code

1. Check if a Client Component is actually required.
2. Verify TypeScript types are proper and strict.
3. Ensure consistent use of `cn()` and design tokens.
4. Look for missing loading/error/empty states.
5. Confirm accessibility basics are covered.
6. Prefer existing design system components over new ones.
7. Keep components small and focused.

## Production Readiness Checklist

- [ ] TypeScript strict mode enabled
- [ ] ESLint + Prettier (or Biome) configured
- [ ] Proper metadata and Open Graph tags
- [ ] Error boundaries in place
- [ ] Loading states implemented
- [ ] Mobile responsiveness verified
- [ ] Core Web Vitals measured
- [ ] Accessibility basic audit passed
- [ ] Bundle size reviewed
