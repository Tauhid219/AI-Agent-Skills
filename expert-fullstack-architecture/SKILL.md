---
name: expert-fullstack-architecture
description: Guides on analyzing and designing software systems, structuring clean codebases (Laravel, Flutter, etc.), drawing architecture diagrams, defining modules, defining boundaries, and reverse-engineering existing codebases into robust architectural patterns.
---

# Expert Full-Stack Software Architecture

This skill provides guidelines and frameworks for analyzing, defining, and reverse-engineering full-stack software architectures—specifically for Laravel (backend/API) and Flutter (mobile frontend).

## Core Principles

1. **System Decomposition & Architecture Mapping**:
   - Break down monolithic systems into clear layers (e.g., Domain, Data, Presentation).
   - Analyze dependencies, design patterns, and entity relations (ERDs).
   - Map files, classes, and namespaces to their corresponding architectural boundaries.

2. **Clean Backend Architecture (Laravel)**:
   - Keep controllers thin by offloading domain logic to **Service Classes**, **Actions**, or **Jobs**.
   - Enforce database decoupling where appropriate by using the Repository Pattern, and use Form Requests for strict validation.
   - Maintain solid data models with proper Eloquent relationships, accessors, mutators, and serialization.

3. **Clean Frontend Architecture (Flutter)**:
   - Enforce the separation of views (UI) and business logic using robust state management (BLoC, Cubit, Riverpod).
   - Define clear Repository contracts (`abstract class`) to isolate the data layer (network requests/local database) from the presentation layer.
   - Leverage dependency injection (e.g., `GetIt`, `Provider`) for decoupled, testable components.

4. **Reverse Engineering**:
   - Read and dissect existing code to document how features work end-to-end.
   - Extract architectural design specifications, database migration definitions, API contracts, and state transition logic.
