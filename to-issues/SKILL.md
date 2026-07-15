---
name: to-issues
description: Converts a PRD, product requirements document, feature spec, or GitHub issue into a Kanban-ready set of independently grabbable implementation issues with vertical slices, tracer-bullet tasks, repo-aware module discovery, acceptance criteria, and blocking relationships. Use when the user invokes /to-issues, asks to break down a PRD, wants a journey from destination to implementation, or needs tasks for parallel agents.
---

# To Issues

Turn a PRD into a journey: a Kanban-ready set of independently grabbable issues. Each issue should be a thin vertical slice through the system, not a horizontal slice of only one layer.

## Workflow

1. Locate the PRD or source requirements. If it is not in the conversation or workspace, ask for it or fetch it if the available tools allow that.
2. Explore the codebase before drafting issues. Identify routes, controllers, models, database tables, services, components, tests, integrations, conventions, and likely ownership boundaries.
3. Draft vertical slices that reveal unknown unknowns quickly. Prefer tracer bullets that touch the UI/API, validation, data flow, business logic, and tests in one narrow end-to-end path.
4. Establish blocking relationships. Mark which issues are unblocked, which depend on earlier learning, and which can run in parallel.
5. Keep each issue independently grabbable: include context, goal, files/modules likely involved, acceptance criteria, test expectations, dependencies, and non-goals.
6. If GitHub tooling is available and the user wants it, create issues and labels/milestones as appropriate. Otherwise, provide GitHub issue-ready Markdown grouped by Kanban status.

## Slicing Rules

Prefer vertical slices over horizontal layers. For example, avoid separate issues like "create database tables", "build controller", and "build UI" unless the work is truly separable. Prefer a narrow task like "Allow admins to create the first draft of X end-to-end" that includes only the schema, route, validation, UI/API, and test coverage needed for that behavior.

Make early issues de-risk integration, data shape, authorization, and user workflow assumptions. Push polish, breadth, bulk migration, optimization, and edge-case expansion into later issues when possible.

## Issue Standards

For each issue, include:

- Title
- Status or lane
- Objective
- User value
- Scope
- Likely files or modules
- Acceptance criteria
- Test expectations
- Dependencies and blockers
- Parallelization notes
- Risks or open questions

Use the template in `resources/issue-template.md` when available.
