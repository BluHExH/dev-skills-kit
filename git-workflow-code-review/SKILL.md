---
name: git-workflow-code-review
description: Use when working with Git, committing changes, creating pull requests, or performing code reviews. Triggers on commit messages, branching strategy, PR description, review comments, and collaboration workflow.
---

# Git Workflow & Code Review

## Commit Message Convention

Use Conventional Commits style:

```
type(scope): short summary

Optional longer description.
```

Common types: feat, fix, docs, style, refactor, test, chore, perf, ci

Examples:
- feat(auth): add password reset flow
- fix(api): handle null response from payment provider
- refactor(ui): extract Button variants into separate components

## Branching Strategy

- `main` / `master` → production-ready code
- `develop` (optional) → integration branch
- Feature branches: `feat/short-description`
- Bugfix branches: `fix/issue-description`
- Hotfix: `hotfix/critical-issue`

Keep branches short-lived. Merge or rebase frequently.

## Pull Request Guidelines

A good PR:
- Has a clear title and description of what and why
- Is relatively small (ideally under 400 lines of meaningful change)
- Includes screenshots or recordings for UI changes
- Links related issues
- Passes all CI checks before review

## Code Review Checklist

Reviewer should check:
1. Does the change solve the intended problem?
2. Is the code readable and maintainable?
3. Are there any obvious bugs or edge cases missed?
4. Is error handling adequate?
5. Are tests added or updated when needed?
6. Does it follow project conventions?
7. Is there any security or performance concern?

## Review Comment Style

- Be kind and constructive.
- Prefer questions over commands when possible.
- Distinguish between "must fix" and "suggestion".
- Approve when the code is good enough, not perfect.
