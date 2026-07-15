---
name: blade-view-guardrails
description: Laravel Blade view safety guidance for Antigravity. Use when editing Blade templates, diagnosing parser errors reported near endforeach, endif, or compiled views, or adding inline PHP such as page titles and temporary variables. Prevent invalid short @php(...) assignment syntax by using safe Blade or PHP block patterns that compile correctly.
---

# Blade View Guardrails

Edit Blade templates in ways that compile predictably. Treat parser errors reported at `@endforeach`, `@endif`, or the bottom of a view as possible upstream syntax failures, especially around inline PHP directives.

## Core Rules

1. Never write assignment or statement-style logic in short `@php(...)` form.
2. Use block PHP for statements:

```blade
@php
    $title = 'Roles Management';
@endphp
```

3. Prefer native Blade constructs over inline PHP when a Blade feature already fits:

```blade
@section('title', 'Roles Management')
<x-page-header :title="'Edit Role: ' . $role->name" />
```

4. Keep `@foreach`, `@if`, `@isset`, and similar directives visually balanced after every edit. Do not assume the reported closing directive is the real bug.

## Unsafe vs Safe Patterns

Avoid:

```blade
@php($title = 'Roles Management')
@php($title = 'Edit Role: '.$role->name)
```

Use one of these instead:

```blade
@php
    $title = 'Roles Management';
    $heading = 'Edit Role: ' . $role->name;
@endphp
```

```blade
@section('title', 'Roles Management')
```

```blade
{{ 'Edit Role: ' . $role->name }}
```

Use short `@php(...)` only for expressions that are already safe in the project's Blade version. If there is any doubt, switch to a block.

## Debugging Workflow

1. Read the lines above the reported `@endforeach` or `@endif` first.
2. Look for inline `@php(...)`, broken concatenation, missing quotes, or half-finished directives.
3. If the error still looks unclear, inspect the compiled Blade output or run the narrowest available Blade or Laravel verification command.
4. After fixing the real syntax issue, re-check loop and conditional balance before concluding the template is clean.

## Edit Checklist

Before finishing a Blade change:

1. Replace any statement-like `@php(...)` with `@php ... @endphp`.
2. Prefer `@section`, props, or plain Blade echoes for titles and simple view data.
3. Scan for misleading downstream error locations around closing directives.
4. Run the narrowest relevant validation available for the project.
