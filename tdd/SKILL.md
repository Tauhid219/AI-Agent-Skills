---
name: tdd
description: Guides Test-Driven Development using a red-green-refactor loop to improve code quality, confirm behavior, design testable interfaces, write one test at a time, implement only enough code to pass, refactor safely, and choose sensible mocking boundaries. Use when the user invokes /tdd, asks for TDD, wants robust implementation, needs tests before code, or wants higher-quality agent output.
---

# TDD

Use Test-Driven Development to make implementation solid. Work in a red-green-refactor loop: write a failing test, implement the smallest meaningful change to pass it, then refactor while keeping tests green.

## Workflow

1. Confirm the interface changes needed before writing tests. Identify public APIs, routes, commands, components, functions, classes, database effects, emitted events, and user-visible behavior.
2. Confirm which behaviors to test. Separate required behavior, edge cases, regressions, authorization, validation, errors, and non-goals.
3. Design interfaces for testability. Prefer deeper modules with thin, stable interfaces over many tiny modules with leaky coordination logic.
4. Write one focused failing test first. Do not write a large batch of speculative tests when the design is still moving.
5. Run the test and verify it fails for the expected reason.
6. Implement only enough code to make that test pass.
7. Run the test again. If it passes, run the nearby relevant tests.
8. Look for refactoring candidates: duplication, unclear names, awkward boundaries, excessive mocking, long methods, leaky abstractions, and behavior split across too many shallow modules.
9. Refactor while tests stay green.
10. Repeat until the behavior is complete.

## Interface Design

Before coding, identify the smallest stable interface that can carry the behavior. Favor modules that hide meaningful complexity behind a simple API. A deep module has a narrow surface and useful internal depth; a shallow module adds names without reducing complexity.

Do not create abstractions just to satisfy a test. Extract modules when they clarify ownership, isolate hard-to-test dependencies, or make behavior easier to reason about.

## Mocking Philosophy

Mock at system boundaries: network calls, payment providers, queues, mailers, clocks, filesystem, random values, process execution, and slow or nondeterministic services.

Avoid mocking the code under test so thoroughly that the test only verifies implementation details. Prefer real domain objects, real validation, real database behavior, and framework test helpers when they give confidence without excessive setup.

## Test Quality

A good test states behavior, not implementation trivia. It should fail for a meaningful regression and be readable enough to act as documentation.

Prefer assertions on observable outcomes: response status and body, database state, emitted jobs/events/notifications, rendered output, return values, logs when relevant, and changed side effects.

Use the checklist in `resources/tdd-checklist.md` for larger implementations.
