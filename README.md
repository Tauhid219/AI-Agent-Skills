# AI Agent Skills

This repository contains a collection of custom skills for AI coding assistants. These skills help the AI understand advanced architectural patterns, domain-driven design, and maintainable software practices.

## Overview

By adding these skills to your AI agent's configuration, you can significantly improve the quality of the generated code. The agent will proactively ask about boundaries, invariants, and testability before writing code, rather than defaulting to tightly-coupled or anemic patterns.

## Available Skills

- **Architecture & DDD**: `application-layer`, `architecture-migration`, `bounded-contexts`, `distribution-patterns`, `domain-events`, `domain-modeling`, `event-sourcing`, `infrastructure-boundaries`, `persistence-patterns`, `dependency-injection`
- **Framework & Backend**: `framework-integration`, `senior-laravel-developer`, `laravel-ui-integrator`
- **Frontend & Mobile**: `flutter-developer`, `blade-view-guardrails`
- **Engineering Practices**: `expert-fullstack-architecture`, `senior-software-engineer`, `production-resilience`, `mock-data-seeder-design`
- **Testing**: `tdd`, `testing-strategy`
- **Productivity & Context**: `context-optimization`, `prompt-deconstruction`, `to-issues`, `to-prd`, `improve-codebase-architecture`, `grill-me`

## Usage
These skills are meant to be placed in your AI Agent's global customization root (e.g., `~/.gemini/config/skills/` or similar tool-specific directories). Once added, the AI will automatically load them and adapt its behavior when relevant contexts or keywords are triggered.
