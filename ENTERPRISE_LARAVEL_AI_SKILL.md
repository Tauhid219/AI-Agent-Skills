---
name: enterprise-laravel-architect
description: Guides and enforces enterprise-grade layered Laravel architecture featuring Service-Repository patterns, Idempotency keys, Transactional Outbox, deterministic ID sequencing, granular RBAC, bilingual schema, Scramble OpenAPI documentation, and Acceptance-Test Driven Development (ATDD).
---

# Enterprise Laravel Architecture Skill

Use this skill when designing, scaffolding, or implementing enterprise-level Laravel applications where maintainability, auditability, data integrity, and strict architectural boundaries are mandatory.

---

## 1. Architectural Philosophy & Layered Design

Applications must NOT follow the naive MVC pattern (Fat Controllers or Fat Models). Instead, strictly enforce a **Layered Domain-Driven Architecture**:

```
Client (Web / Mobile / Third-party)
        │
        ▼
[HTTP Layer] ──> Form Requests (Validation & Authorization)
        │
        ▼
[Controllers] ──> Thin HTTP Orchestrators (HTTP status, Request payload -> Service -> API Resource)
        │
        ▼
[Service Layer] ──> Business Logic, Domain Invariants, Transactions, Events, Audit Logging
        │
        ▼
[Repository Layer] ──> Database Query Abstraction, Eloquent query encapsulation, Deterministic ID generation
        │
        ▼
[Domain Models & Enums] ──> Schema representation, Relationships, State machines, Casts
        │
        ▼
[Database / Storage] ──> PostgreSQL / MySQL / SQLite with foreign keys, indexes, and JSON columns
```

### Layer Rules:
1. **Controllers (`app/Http/Controllers/`)**:
   - MUST remain thin (< 50 lines per method).
   - Only responsible for accepting requests, invoking one or two service methods, and returning API Resources.
   - NEVER execute direct Eloquent write queries (`Model::create`, `DB::table`) or raw transaction closures inside controllers.
2. **Form Requests (`app/Http/Requests/`)**:
   - Every POST, PUT, PATCH request MUST have a dedicated FormRequest.
   - Enforce authorization policies inside `authorize()` or route middleware.
   - Provide bilingual validation messages (`messages()` method supporting Arabic/Bangla/English depending on domain).
3. **Services (`app/Services/<Domain>/`)**:
   - Encapsulate all business rules, eligibility checks, state transitions, and calculations.
   - Group services by Bounded Context (e.g., `app/Services/Academics/`, `app/Services/Admissions/`, `app/Services/Promotions/`, `app/Services/AuditTrail/`).
   - Use Database Transactions (`DB::transaction`) inside services whenever writing across multiple tables.
4. **Repositories (`app/Repositories/<Domain>/`)**:
   - Centralize reusable database reads, complex queries, and entity persistence.
   - Abstract custom sequence generation (e.g., `IdSequenceRepository`).
5. **API Resources (`app/Http/Resources/<Domain>/`)**:
   - NEVER return raw Eloquent models directly to clients.
   - Always transform output through API Resources.
   - Format timestamps in ISO 8601 (`$this->created_at?->toIso8601String()`).
   - Transform enum instances into safe scalar keys and human-readable bilingual labels.

---

## 2. Core Enterprise Design Patterns

### Pattern A: Deterministic Public ID Sequencing
Never expose auto-increment integer primary keys (`id`) in URLs or public API contracts.
- **Implementation**:
  - Internal database relations use fast integer `id` (bigIncrements).
  - External exposure uses formatted public IDs: e.g., `public_id` column (`usr_001`, `std_2026_0012`).
  - Maintain dedicated sequence tables (`id_sequences`, `student_id_sequences`) with row-locking (`lockForUpdate()`) to guarantee zero collision even under high concurrency.

### Pattern B: Transactional Outbox Pattern
When critical state changes must trigger notifications, events, or third-party webhooks:
- Do NOT directly dispatch asynchronous jobs that could succeed if the surrounding database transaction rolls back.
- Persist the event record (e.g., `enrollment_outbox`, `promotion_events`) inside the same atomic database transaction as the state mutation.
- A reliable worker/listener processes the outbox queue idempotently.

### Pattern C: Idempotency Protection
Critical financial mutations, batch operations, and admission enrollments MUST be protected against duplicate submissions, double-clicks, and network retries:
- Accept a header or payload key: `X-Idempotency-Key` or `client_mutation_id`.
- Check and store mutation states in dedicated tables (`promotion_idempotency_keys`, `enrollment_request_keys`).
- If a duplicate key arrives while the operation is in progress, return HTTP `409 Conflict`.
- If a completed operation receives the same key, return the cached previous response body with HTTP `200 OK`.

### Pattern D: Centralized Audit Trail
Every non-trivial administrative or financial mutation MUST be recorded:
- Dedicated `AuditLogger` service.
- Record: `user_id`, `action`, `entity_type`, `entity_id`, `before_metadata` (JSON), `after_metadata` (JSON), `ip_address`, `user_agent`.
- Store both localized and system-level event descriptions.

### Pattern E: Type-Safe Enums & Domain State Machines
- Replace string/integer constants with PHP 8.1+ String-Backed Enums (`app/Enums/`).
- Enums must provide helper methods for localized labels (e.g., `labelEn()`, `labelBn()`).
- Implement state validation on transitions (e.g., `DRAFT -> VALIDATED -> PROCESSING -> COMPLETED`).

### Pattern F: Custom Domain Exceptions
- Avoid generic 500 crashes.
- Create domain-specific exceptions (e.g., `AcademicDomainException`, `AdmissionsDomainException`, `PromotionDomainException`).
- Define semantic error codes: `INSUFFICIENT_CAPACITY`, `SCHEME_MISMATCH`, `DUPLICATE_REGISTRATION`.
- Map domain exceptions to appropriate HTTP status codes (400, 403, 404, 409, 422) in `bootstrap/app.php` or `app/Exceptions/Handler.php`.

---

## 3. Database & Schema Design Guidelines

1. **Database Agnostic Migrations**:
   - Write migrations that run seamlessly across **MySQL / MariaDB**, **PostgreSQL**, and **SQLite**.
   - Use standard `$table->json('payload')` or `$table->jsonb('payload')` (Laravel automatically falls back to `json` on MySQL and SQLite).
   - Use composite indexes on frequently filtered column pairs (e.g., `program_id` + `status`, `student_id` + `academic_period_id`).
   - Define strict foreign key constraints with explicit `onDelete('restrict')` or `cascadeOnDelete()`.
2. **Bilingual Schema Attributes**:
   - When building multilingual systems (e.g., Bengali & English or Arabic & English):
   - Always name fields symmetrically: `name_bn`, `name_en`; `description_bn`, `description_en`.
   - Provide a database-agnostic fallback in model accessors or display helpers.
3. **Idempotent Seeders**:
   - Seeders must use `updateOrCreate(['key' => ...], [...])` instead of raw `insert()` so running `db:seed` multiple times never corrupts or duplicates records.

---

## 4. Security & Role-Based Access Control (RBAC)

1. **Authentication**:
   - Use **Laravel Sanctum** for both SPA session-based authentication and Mobile API bearer tokens.
   - Enforce an account activation flow (`/api/v1/auth/activate`) using secure tokens and expiration windows.
2. **Granular RBAC**:
   - Three standard tier roles: `ADMIN`, `OFFICE_STAFF`, `ACCOUNTS`.
   - Permissions must be discrete dot-notated actions (e.g., `students.read`, `students.create`, `admissions.reallocate`, `promotions.batches.execute`).
   - Enforce permissions via route middleware: `middleware('permission:students.create')`.
   - Provide field-level or action-level access levels: `ALLOWED`, `RESTRICTED`, `VIEW_ONLY`, `OWN_ONLY`.
3. **Password Security**:
   - Minimum 12 characters.
   - Require uppercase, lowercase, numbers, and symbols.
   - Secure hash with bcrypt rounds (>= 12 in production, 4 in testing).

---

## 5. API Documentation & OpenAPI Standards

- Integrate **Dedoc Scramble** (`dedoc/scramble`) for automated, zero-maintenance OpenAPI 3.0 interactive documentation.
- All endpoints must be documented at `/docs/api` with full query parameters, payload examples, and error schemas.
- Document PHP docblocks on Controller methods and FormRequest attributes to auto-enrich Scramble's generated specifications.

---

## 6. Acceptance-Test Driven Development (ATDD)

1. **Deliverable-Based Test Suites**:
   - Organize feature tests around business deliverables (e.g., `DeliverableOneComprehensiveTest`, `DeliverableTwoComprehensiveTest`).
   - Label test methods with specific Acceptance Test codes: `AT-D02-01`, `AT-D02-02`.
2. **Testing Best Practices**:
   - Use `RefreshDatabase` or `DatabaseTransactions` to maintain clean test isolation.
   - Test both happy paths and strict adversarial boundary conditions:
     - Anonymous access returns `401 Unauthorized`.
     - Unauthorized role returns `403 Forbidden`.
     - Invalid payload returns `422 Unprocessable Content`.
     - State conflict returns `409 Conflict`.
     - Over-capacity or duplicate allocation is strictly prevented.
   - Mock only external boundaries (Email, SMS gateway, Cloud Storage). Test database queries, transactions, and service logic against a real test database.

---

## 7. Command-Line Tooling & Bootstrap Commands

Always provide custom Artisan console commands for critical lifecycle maintenance:
- `php artisan <app>:admin:bootstrap` with options for `--username`, `--email`, `--password`, `--name-en`, `--name-bn`.
- Command must automatically verify required roles exist, generate a deterministic public ID, hash the password, and attach system administrator permissions.

