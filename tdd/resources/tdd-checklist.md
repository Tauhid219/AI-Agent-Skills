# TDD Checklist

Use this checklist while implementing with red-green-refactor.

## Before the First Test

- Identify the user-visible behavior or public interface being changed.
- Confirm non-goals so tests do not lock in unwanted behavior.
- Choose the test level: unit, feature, integration, browser, API, command, job, or component.
- Identify boundaries to fake or mock: external APIs, time, random values, filesystem, queues, mail, notifications, and slow services.

## Red

- Write one focused test.
- Name the test after the behavior it protects.
- Run it and confirm it fails for the expected reason.
- If it fails for setup noise, fix the setup before implementing production code.

## Green

- Implement the smallest meaningful code path that satisfies the test.
- Avoid speculative abstractions.
- Preserve existing behavior unless the user requested a change.
- Run the new test until it passes.

## Refactor

- Remove duplication introduced by the change.
- Improve names and boundaries if they reduce cognitive load.
- Move logic behind a stable interface when it hides real complexity.
- Delete unused code paths only when they are clearly obsolete and inside the task scope.
- Run the relevant test set after refactoring.

## Completion

- Add edge-case tests for important validation, authorization, errors, empty states, and regressions.
- Run the narrowest meaningful suite plus any affected neighboring tests.
- Summarize what behavior is now protected.
