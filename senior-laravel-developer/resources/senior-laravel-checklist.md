# Senior Laravel Checklist

Use this checklist for audits, broad debugging, refactors, and production-readiness reviews.

## Architecture

- Follow existing project structure before introducing services, actions, repositories, DTOs, or modules.
- Keep controllers focused on HTTP concerns: authorization, validation orchestration, calling domain/application logic, and returning responses.
- Put reusable business rules near the domain model, service/action layer, policy, job, listener, or observer according to local convention.
- Avoid hidden side effects in accessors, casts, observers, and model events unless the codebase already relies on them deliberately.

## Routing and HTTP

- Confirm route middleware, auth guards, throttling, route model binding, names, prefixes, and API versioning.
- Preserve existing redirects, flashed messages, JSON structures, and status codes unless a change is requested.
- Use explicit authorization for sensitive resources before exposing model data.

## Validation

- Use Form Requests for complex validation and inline validation for small local patterns.
- Validate file uploads by MIME, size, dimensions when relevant, and storage visibility.
- Validate IDs against the correct tenant/user scope, not only table existence, when data isolation matters.

## Eloquent and Database

- Define relationships with correct cardinality, keys, pivot table names, timestamps, casts, and soft delete behavior.
- Prefer query constraints in SQL over filtering large collections in PHP.
- Add indexes for foreign keys, lookup columns, uniqueness constraints, sortable/filterable fields, and queue/reporting hot paths.
- Use transactions for multi-write operations that must succeed or fail together.
- Be careful with mass assignment, guarded/fillable, hidden attributes, appends, and serialization.


## Modern PHP Patterns

- Match the project's minimum PHP version before using newer syntax.
- Prefer explicit types, enums, value objects, DTOs, readonly data, and match expressions when they reduce invalid states or ambiguity.
- Avoid adding interfaces, traits, abstract classes, or DTOs when they only add ceremony.
- Keep Pint/PHP-CS-Fixer, PHPStan/Larastan, and project style expectations in mind.

## API Design

- Preserve response envelopes, status codes, pagination shape, error format, and versioning conventions.
- Use Form Requests, API Resources, route model binding, policies, and consistent exceptions when established locally.
- Cover request validation, authorization failures, filtering/sorting/search, pagination, idempotency, and JSON response contracts.
- Consider OpenAPI/client contract updates when public API behavior changes.

## Frontend Integrations

- Inspect Blade, Livewire, Inertia, Vue, React, Alpine, Vite, Tailwind, localization, and frontend entry points before UI-affecting changes.
- Preserve validation error rendering, flashed state, redirects, props, component state, and API/frontend contracts.
- Consider accessibility, loading states, empty states, authorization-driven visibility, filters, pagination state, and browser coverage.

## Security

- Check policies, gates, guards, middleware, signed URLs, CSRF, CORS, rate limits, and tenant boundaries.
- Avoid logging secrets, tokens, passwords, PII, raw request bodies, or payment data.
- Treat raw SQL, dynamic order-by fields, file paths, redirects, and webhooks as injection surfaces.

## Queues, Events, and Scheduling

- Move slow email, notification, import/export, webhook, and third-party API work to queues when appropriate.
- Check retry/backoff/idempotency for jobs that call external services or write important data.
- For scheduled tasks, consider overlapping locks, timezone, failure visibility, and queue worker availability.

## Testing

- Cover authorization failures, validation failures, happy paths, and key regressions.
- Use factories instead of brittle fixture setup when possible.
- Prefer feature tests for routes/controllers/API behavior and unit tests for pure domain logic.
- Assert database changes, emitted jobs/events/notifications, rendered views/resources, and response JSON where relevant.

## Performance and Reliability

- Look for N+1 queries, unbounded `all()` calls, missing pagination, missing eager loading, heavy sync tasks, and cache stampedes.
- Consider config/route/view cache, queue workers, storage links, migrations, and deployment order for production readiness.
- Preserve backward compatibility for public APIs, jobs already on queues, serialized notifications, and migrations deployed to shared environments.


