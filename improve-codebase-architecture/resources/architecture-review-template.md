# Architecture Review Template

Use this template when presenting codebase architecture improvement candidates.

## Summary

Describe the main architectural theme: unclear ownership, shallow modules, scattered concepts, coupling, weak test boundaries, or integration risk.

## Candidate: [Title]

**Current Confusion:**
Describe where the reader has to jump between files, understand hidden coordination, or infer ownership.

**Why It Matters:**
Explain how this causes bugs, weak tests, slow changes, unclear agent navigation, or risky implementation.

**Deepening Opportunity:**
Describe how to move complexity behind a narrower, more useful interface.

**Proposed Interface:**
Show the intended public method, class, command, route, service, component API, or module boundary.

**Affected Files or Modules:**
- `path/or/module`
- `path/or/module`

**Suggested Test Boundary:**
Recommend unit, feature, integration, API, browser, command, job, or component tests.

**Migration Plan:**
1. Small first step
2. Follow-up step
3. Cleanup step

**Risks and Rollback:**
Describe compatibility risks, deployment concerns, and how to back out safely.

## Prioritization

Rank candidates by impact, confidence, effort, and ability to unlock safer TDD.
