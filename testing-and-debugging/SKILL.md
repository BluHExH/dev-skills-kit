---
name: testing-and-debugging
description: Use when writing tests, improving test coverage, or systematically debugging issues. Triggers on unit tests, integration tests, end-to-end tests, test strategy, debugging process, and fixing bugs methodically.
---

# Testing and Debugging

## Testing Strategy

Aim for a balanced pyramid:
- Many fast unit tests
- Fewer integration tests
- Small number of critical end-to-end tests

Test behavior, not implementation details.
Prefer testing public APIs and user-visible outcomes.

## Unit Testing Guidelines

- Write tests that are readable and act as documentation.
- Follow Arrange-Act-Assert (or Given-When-Then).
- Keep tests independent and deterministic.
- Mock only what is necessary (external services, time, randomness).
- Avoid testing private methods directly.

## Integration & E2E

- Integration tests should verify real collaboration between modules.
- E2E tests should cover critical user flows only.
- Keep E2E tests stable by using reliable selectors and avoiding flakiness.

## Systematic Debugging Process

1. Reproduce the bug reliably.
2. Form a hypothesis about the root cause.
3. Gather evidence (logs, breakpoints, network, state).
4. Narrow the scope (binary search through the code path).
5. Fix the root cause, not just the symptom.
6. Add a regression test when possible.
7. Verify the fix and check for side effects.

## Debugging Tips

- Read the full error message and stack trace carefully.
- Check recent changes first (git blame / recent commits).
- Log intermediate values instead of guessing.
- Use the debugger rather than excessive console.log when possible.
- Confirm assumptions about data shape and types.
- When stuck, explain the problem out loud or write it down (rubber duck).

## When Writing Tests for New Features

- Cover the happy path.
- Cover important edge cases and error paths.
- Ensure tests fail for the right reason before implementing the feature (TDD optional but useful).
