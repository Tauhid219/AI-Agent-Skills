---
name: to-prd
description: Converts a conversation, rough idea, or shared understanding into a Product Requirements Document with repo verification, follow-up questioning, module sketching, user stories, acceptance criteria, and a GitHub issue-ready format. Use when the user invokes /to-prd, asks for a PRD, wants to turn an idea or conversation into requirements, or needs product requirements before implementation.
---

# To PRD

Create a Product Requirements Document from the conversation and available repository context. Skip steps that are already satisfied by the conversation, but do not skip repo exploration when assertions can be verified locally.

## Workflow

1. Ask the user for a detailed description if the goal, users, constraints, or desired behavior are still unclear.
2. Explore the repository to verify assertions about the current system, naming, architecture, routes, data models, UI surfaces, tests, and integration points.
3. Interview the user relentlessly using the `grill-me` posture when decisions remain unresolved. Walk the design tree until the major branches are understood.
4. Sketch the major modules, files, components, data changes, APIs, workflows, jobs, events, permissions, and external integrations likely involved.
5. Write the PRD using the template in `resources/prd-template.md` when available.
6. If GitHub tooling is available and the user wants it, submit the PRD as a GitHub issue. Otherwise, provide the issue-ready Markdown.

## PRD Standards

Prioritize user stories. Write them in plain language that describes desired system behavior from the user's perspective. Include acceptance criteria that can guide implementation and testing.

Keep requirements separate from implementation notes. Include implementation context only where it reduces ambiguity for engineers.

Call out open questions, risks, dependencies, non-goals, rollout concerns, and test expectations. If a requirement is inferred rather than confirmed, mark it as an assumption.
