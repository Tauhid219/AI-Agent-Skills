# AI Agent Skills

A collection of **34 custom skills** for AI coding agents: Claude Code, Gemini CLI / Antigravity, OpenAI Codex, and any other agent that reads `SKILL.md` files.

The skills cover Laravel/PHP architecture, Domain-Driven Design, frontend (React, Vue, Tailwind), Flutter, testing, and product/planning workflows. With a skill installed, the agent follows proven patterns and asks about boundaries, invariants, edge cases and testability before it writes code. Without them, agents tend to fall back to fat controllers, anemic models and happy-path-only code.

---

## Table of contents

- [What is a skill?](#what-is-a-skill)
- [Available skills](#available-skills)
- [Installation](#installation)
  - [Claude Code](#claude-code)
  - [Gemini CLI / Antigravity](#gemini-cli--antigravity)
  - [OpenAI Codex](#openai-codex)
  - [One repo, many agents (symlinks)](#one-repo-many-agents-symlinks)
- [Usage](#usage)
- [Repository structure](#repository-structure)
- [Writing a new skill](#writing-a-new-skill)
- [Credits & licensing](#credits--licensing)

---

## What is a skill?

A skill is a folder containing a `SKILL.md` file. The file starts with YAML frontmatter, followed by Markdown instructions:

```markdown
---
name: tdd
description: Guides Test-Driven Development using a red-green-refactor loop... Use when the user invokes /tdd, asks for TDD, ...
---

# Instructions the agent follows when the skill is active
...
```

The agent only sees each skill's `name` and `description` up front. When a task matches the description, it loads the full instructions, plus any extra files in `references/` or `resources/`. Skills stay out of the context window until they are needed.

---

## Available skills

### 🏛️ Architecture & Domain-Driven Design (PHP)

| Skill | What it does |
|---|---|
| `domain-modeling` | Choosing between Transaction Script, Table Module and a rich Domain Model, and when full DDD is worth its cost. |
| `bounded-contexts` | Drawing system boundaries around business capabilities, shared kernels and context maps in a PHP monolith. |
| `application-layer` | Thin controllers, Service/Application Layer, and Command/Query/Handler (CQRS) use cases with DTOs. |
| `distribution-patterns` | Coarse-grained Remote Facades and DTOs for REST/GraphQL/gRPC, so ORM entities don't leak into APIs. |
| `persistence-patterns` | Data-source patterns, repositories, mappers, and splitting oversized repositories. |
| `infrastructure-boundaries` | Adapters for third-party APIs/SDKs, webhook handling, and avoiding vendor lock-in. |
| `domain-events` | Record-then-publish domain events for decoupling side-effects and cross-context communication. |
| `event-sourcing` | When Event Sourcing is (and isn't) appropriate; event stores, projections, snapshots and PHP libraries. |
| `dependency-injection` | Constructor injection, container wiring, and migrating away from service locators. |
| `architecture-migration` | Incremental refactoring of legacy code with vertical slices, characterization tests and temporary shims. |
| `framework-integration` | Mapping clean/hexagonal architecture onto Laravel, Symfony, Slim, CakePHP, CodeIgniter, Yii, Laminas, Mezzio and more. |
| `testing-strategy` | The right test style (unit, integration, feature) for each architectural layer. |

### 🔶 Laravel

| Skill | What it does |
|---|---|
| `senior-laravel-developer` | Senior-level Laravel guidance: Eloquent, migrations, validation, auth/policies, queues, events, APIs, performance, security and deployment readiness. |
| `enterprise-laravel-architect` | Enterprise layered architecture: Service-Repository, idempotency keys, Transactional Outbox, deterministic IDs, granular RBAC, bilingual schema, Scramble OpenAPI and ATDD. |
| `blade-view-guardrails` | Prevents Blade parser errors (for example, the invalid short `@php(...)` assignment syntax) and diagnoses errors reported near `endforeach`/`endif`. |
| `laravel-ui-integrator` | Converts static HTML/Bootstrap/Tailwind templates into Blade, React, Vue or Inertia.js layouts and components. |
| `mock-data-seeder-design` | Realistic seeders, factories and mock data for Laravel, Flutter mock services and API contracts. |

### 🎨 Frontend

| Skill | What it does |
|---|---|
| `senior-javascript-developer` | Modern JavaScript (ES6+) and TypeScript: async patterns, performance and architecture. |
| `senior-react-developer` | React 18+, hooks, component architecture, state management and Next.js. |
| `senior-vue-developer` | Vue 3 Composition API, reactivity, Pinia, routing and Nuxt.js. |
| `tailwindcss-expert` | Utility-first patterns, responsive layouts, custom themes and framework integration. |

### 📱 Mobile (Flutter)

| Skill | What it does |
|---|---|
| `flutter-developer` | Widgets, state management (Bloc, Riverpod, Provider), routing, assets, API integration and responsive design. |
| `flutter-android-build-optimization` | Release-ready APK/AAB builds: avoiding accidental debug builds and fat APKs, release signing, and Play Store preparation. |

### 🧠 Engineering practice

| Skill | What it does |
|---|---|
| `senior-software-engineer` | Language-agnostic senior judgment: planning, debugging, refactoring, code review, security, observability and trade-offs. |
| `expert-fullstack-architecture` | System design, module boundaries, architecture diagrams and reverse-engineering existing codebases. |
| `improve-codebase-architecture` | Finds shallow modules, tight coupling and unclear test boundaries, then proposes ways to deepen modules. |
| `production-resilience` | Exception handling, logging, retries, fallbacks, validation and security guardrails. |
| `adversarial-thinking` | A "code breaker" mindset: builds an Edge Case Matrix (null, empty, negative, duplicate, unexpected types) before any code is written. |

### ✅ Testing

| Skill | What it does |
|---|---|
| `tdd` | Red-green-refactor, one test at a time, with sensible mocking boundaries. |

*(See also `testing-strategy` and `adversarial-thinking` above.)*

### 📋 Planning & productivity

| Skill | What it does |
|---|---|
| `grill-me` | Interviews you relentlessly to flesh out an idea or plan before any code is written. |
| `to-prd` | Turns a conversation or rough idea into a Product Requirements Document with user stories and acceptance criteria. |
| `to-issues` | Breaks a PRD or spec into independently grabbable, vertically sliced implementation issues. |
| `context-optimization` | Structures prompts and repositories so LLMs use their context window efficiently. |
| `prompt-deconstruction` | Reverse-engineers an existing codebase into step-by-step prompts that another AI can use to rebuild it. |

---

## Installation

Clone the repository once:

```bash
git clone https://github.com/Tauhid219/AI-Agent-Skills.git
```

Then copy (or symlink, see below) the skill folders you want into your agent's skills directory. You can install all of them, or only the ones you need.

### Claude Code

Skills live in `~/.claude/skills/<skill-name>/SKILL.md` (all projects) or `<project>/.claude/skills/<skill-name>/SKILL.md` (a single project).

```bash
# macOS / Linux: install every skill
mkdir -p ~/.claude/skills
for d in AI-Agent-Skills/*/; do cp -r "$d" ~/.claude/skills/; done
```

```powershell
# Windows PowerShell: install every skill
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Get-ChildItem AI-Agent-Skills -Directory | Copy-Item -Destination "$HOME\.claude\skills" -Recurse
```

Start a new Claude Code session. The skills appear in the available-skills list, and you can call one directly with `/<skill-name>` (for example `/grill-me` or `/tdd`).

### Gemini CLI / Antigravity

Place the skill folders in `~/.gemini/config/skills/`. The simplest way is to clone the repo straight into that path:

```bash
git clone https://github.com/Tauhid219/AI-Agent-Skills.git ~/.gemini/config/skills
```

### OpenAI Codex

Place the skill folders in `~/.codex/skills/`:

```bash
mkdir -p ~/.codex/skills
for d in AI-Agent-Skills/*/; do cp -r "$d" ~/.codex/skills/; done
```

### One repo, many agents (symlinks)

If you use several agents, keep one clone as the single source of truth and link each skill into the other agents' folders. An edit in the repo then reaches every agent immediately.

```bash
# macOS / Linux
for d in ~/.gemini/config/skills/*/; do
  ln -s "$d" ~/.claude/skills/"$(basename "$d")"
done
```

```powershell
# Windows: directory junctions need no admin rights or Developer Mode
Get-ChildItem "$HOME\.gemini\config\skills" -Directory | Where-Object { Test-Path "$($_.FullName)\SKILL.md" } | ForEach-Object {
  New-Item -ItemType Junction -Path "$HOME\.claude\skills\$($_.Name)" -Target $_.FullName
}
```

> ⚠️ Editing or deleting files *inside* a linked folder changes the original. To remove only the link on Windows, use `rmdir "<link path>"`, not `Remove-Item -Recurse`.

---

## Usage

Most skills activate automatically when your request matches their `description`. For example:

- *"Refactor this controller, it's doing too much"* → `application-layer`, `senior-laravel-developer`
- *"Integrate the Stripe SDK"* → `infrastructure-boundaries`
- *"Build a release APK"* → `flutter-android-build-optimization`

Some skills are designed to be invoked explicitly:

| Command | Result |
|---|---|
| `/grill-me` | The agent questions you until the idea is fully specified. |
| `/to-prd` | Writes a PRD from the current conversation. |
| `/to-issues` | Breaks a PRD into implementation issues. |
| `/tdd` | Implements the next feature test-first. |
| `/improve-codebase-architecture` | Audits the repo and proposes improvements. |

A typical end-to-end workflow: **`/grill-me` → `/to-prd` → `/to-issues` → `/tdd`** for each issue.

---

## Repository structure

```
AI-Agent-Skills/
├── README.md
├── ENTERPRISE_LARAVEL_AI_SKILL.md      # standalone copy of the enterprise-laravel-architect skill
└── <skill-name>/
    ├── SKILL.md                        # required: frontmatter + instructions
    ├── references/ or resources/       # optional: checklists, templates, deep-dive docs
    └── agents/openai.yaml              # optional: Codex UI metadata (ignored by other agents)
```

---

## Writing a new skill

1. Create a folder whose name matches the skill's `name` (lowercase, kebab-case).
2. Add `SKILL.md` with `name` and `description` in the frontmatter.
3. Write a specific `description`: say what the skill does **and when to use it** (keywords, file types, commands). Agents decide whether to load a skill from this text alone.
4. Keep `SKILL.md` focused. Move long checklists or templates into `references/` and link to them from `SKILL.md`.
5. Stay agent-neutral: avoid wording or tool names tied to one specific agent, so the skill works everywhere.

Contributions are welcome. Open an issue or a pull request.

---

## Credits & licensing

The following 12 skills are taken from **[garrettw/php-arch-skills](https://github.com/garrettw/php-arch-skills)** and are licensed under the **[Mozilla Public License 2.0](https://www.mozilla.org/en-US/MPL/2.0/)**:

`application-layer`, `architecture-migration`, `bounded-contexts`, `dependency-injection`, `distribution-patterns`, `domain-events`, `domain-modeling`, `event-sourcing`, `framework-integration`, `infrastructure-boundaries`, `persistence-patterns`, `testing-strategy`

Under MPL-2.0, those files remain under that license, and any modifications to them must also be made available under MPL-2.0. You can also install the originals directly with `npx skills add garrettw/php-arch-skills`.
