---
name: production-resilience
description: Implements robust exception handling, logging, retry policies, fallback behaviors, validation, and security guardrails to ensure production resilience in Laravel, Flutter, and APIs.
---

# Exception Handling & Production Resilience

This skill outlines guidelines to ensure that backend and frontend systems fail gracefully, log exceptions accurately, protect sensitive data, and provide smooth user recovery mechanisms.

## Resilience Architecture

1. **Structured Exception Handling**:
   - Wrap unpredictable operations (network requests, database transactions, file system interactions) in appropriate try-catch blocks.
   - Enforce distinct actions depending on the error type (e.g., network timeout vs. authorization error).

2. **Backend Resilience (Laravel)**:
   - Customize `app/Exceptions/Handler.php` (or use the bootstrap configuration in modern Laravel versions) to return standardized JSON error responses for API calls.
   - Use DB Transactions (`DB::transaction`) to ensure atomicity during multi-step database writes, rolling back completely on failure.
   - Log errors via Laravel's `Log` facade with appropriate levels (debug, info, warning, error, critical) and trace details, masking sensitive user data.

3. **Frontend Resilience (Flutter)**:
   - Handle network failures gracefully in Flutter by writing Dio/Http interceptors to retry requests on timeout or refresh expired tokens.
   - Design fallback screens and UI state indicators (e.g., offline alert banners, retry buttons, localized error text).
   - Use `ErrorWidget.builder` to override Flutter's default red screen of death in production with a custom user-friendly UI.
   - Log errors to cloud monitors (e.g., Sentry, Firebase Crashlytics) while preserving user privacy.

4. **Input Validation & Sanitization**:
   - Enforce strong input validation constraints at system entry points (Form Requests in Laravel, Form/TextFormField validators in Flutter) to block malformed data before execution.
