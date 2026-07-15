---
name: improve-codebase-architecture
description: Explores a codebase to find architecture confusions, unclear test boundaries, shallow modules, excessive file-hopping, test-only pure function extraction, tight coupling, and integration risk, then proposes deepening opportunities that make the codebase easier for agents and humans to understand, test, and change. Use when the user invokes /improve-codebase-architecture, asks to improve architecture, make a codebase agent-friendly, clarify module boundaries, or prepare for TDD.
---

# Improve Codebase Architecture

Explore the codebase naturally and identify architecture changes that would make it easier for agents and humans to understand, test, and modify. Focus on module boundaries, conceptual clarity, and integration risk.

## Workflow

1. Map the current system by following real user-facing flows, commands, jobs, APIs, or features from entry point to persistence and side effects.
2. Look for confusion points: places where understanding one concept requires bouncing between many small files, layers, helpers, and indirections.
3. Identify shallow modules: modules with broad or leaky interfaces that hide little complexity and force callers to coordinate behavior manually.
4. Identify deepening opportunities: places where related behavior can move behind a narrower, more useful interface.
5. Check test boundaries. Note where it is unclear whether behavior should be tested at unit, feature, integration, browser, API, job, or command level.
6. Find pure functions extracted only for testability when the real bugs are in orchestration, integration, state, permissions, or data flow.
7. Find tightly coupled modules where changes in one concept create hidden integration risk elsewhere.
8. Present candidates ranked by impact, risk, and effort. Include a small migration path and test strategy for each candidate.

## What To Look For

- One concept scattered across many files with no obvious owner.
- Callers repeatedly assembling the same multi-step behavior.
- Thin wrappers that add names but not clarity.
- Shared helpers that mix unrelated concepts.
- Validation, authorization, data access, side effects, and formatting tangled together.
- Tests that mock too much and miss real behavior.
- Tests that can only exercise behavior through brittle setup.
- Modules that require deep knowledge of their internals to call correctly.
- Hidden dependencies on global state, config, time, auth context, filesystem, queues, or external services.

## Output Format

For each candidate, include:

- Candidate title
- Current confusion
- Why it matters
- Deepening opportunity
- Proposed interface
- Affected files or modules
- Suggested test boundary
- Migration plan
- Risk and rollback notes

Use `resources/architecture-review-template.md` for broader reviews.
