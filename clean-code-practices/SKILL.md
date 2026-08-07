---
name: clean-code-practices
description: Use when writing, reviewing, or refactoring code for quality. Triggers on clean code, refactoring, code smells, naming conventions, function design, and maintainable software practices.
---

# Clean Code Practices

## Core Rules

- Names should reveal intent. A good name removes the need for a comment.
- Functions should do one thing and do it well.
- Keep functions short (ideally under 20-30 lines).
- Prefer early returns over deep nesting.
- Avoid magic numbers and strings. Extract them as named constants.
- Don't repeat yourself (DRY), but don't force abstraction too early (Rule of Three).

## Naming Guidelines

- Variables and functions: camelCase or snake_case depending on language convention.
- Classes and components: PascalCase.
- Constants: UPPER_SNAKE_CASE or language equivalent.
- Boolean names: is, has, can, should prefixes.
- Avoid abbreviations unless they are universally known.

## Function Design

- Prefer pure functions when possible.
- Limit the number of parameters (ideally ≤ 3). Use objects for more.
- Separate commands (side effects) from queries (return values).
- Handle errors explicitly. Don't swallow exceptions silently.

## Comments

- Prefer self-documenting code over comments.
- Comments should explain "why", not "what".
- Delete commented-out code. Version control exists for a reason.
- Keep TODO comments actionable and temporary.

## When Reviewing Code

1. Can I understand this function without reading its implementation in detail?
2. Are there any long parameter lists or deeply nested conditionals?
3. Is there duplicated logic that should be extracted?
4. Are edge cases and error paths handled?
5. Does the code match the project's existing style and conventions?
