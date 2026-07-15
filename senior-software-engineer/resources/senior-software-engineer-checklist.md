# Senior Software Engineer Checklist

Use this checklist for broad codebase work, architecture review, production readiness, or implementation planning.

## Discovery

- Identify language, framework, package manager, runtime versions, build scripts, test scripts, and deployment hints.
- Read relevant entry points, call sites, tests, config, schema, routes, commands, jobs, and UI/API surfaces.
- Check the current working tree and preserve unrelated user changes.
- Understand existing naming, structure, error handling, logging, and test conventions before editing.

## Requirements

- Clarify user-visible behavior, non-goals, compatibility constraints, data constraints, and rollout expectations.
- Convert ambiguity into assumptions, questions, tests, or codebase inspection.
- Separate required behavior from implementation preferences.

## Design

- Choose cohesive ownership for new behavior.
- Keep interfaces narrow and useful.
- Avoid shallow wrappers and unnecessary layers.
- Consider data model, state transitions, permissions, concurrency, idempotency, and failure modes.
- Plan migrations and compatibility for deployed systems.

## Implementation

- Keep edits scoped and reversible.
- Use existing helpers, framework APIs, and project conventions.
- Handle validation, errors, empty states, boundaries, and edge cases.
- Avoid new dependencies unless the benefit clearly outweighs cost.
- Keep comments sparse and useful.

## Testing

- Pick the right test level for the behavior.
- Cover happy path, important edge cases, validation, authorization, and regressions.
- Assert observable outcomes: returned values, response shape, database state, emitted events/jobs, rendered UI, logs, or side effects.
- Run targeted tests first, then broader checks when risk justifies it.

## Security

- Check auth, authorization, input validation, injection, file paths, redirects, SSRF, secrets, logs, dependency risk, and data isolation.
- Avoid exposing sensitive data in errors, logs, responses, tests, or fixtures.

## Performance and Reliability

- Watch for unbounded queries, N+1s, blocking I/O, memory growth, missing indexes, cache invalidation issues, race conditions, flaky retries, and missing timeouts.
- Consider queues, scheduled tasks, health checks, feature flags, rollbacks, and operational visibility.

## Review and Delivery

- Explain what changed and why.
- Mention verification performed and anything not run.
- Call out residual risks, assumptions, and follow-up work only when meaningful.
