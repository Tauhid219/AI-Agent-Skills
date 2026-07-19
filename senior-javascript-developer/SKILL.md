---
name: senior-javascript-developer
description: Provides senior-level guidance for modern JavaScript (ES6+) and TypeScript development. Focuses on advanced patterns, async programming, performance optimization, and robust architecture.
---
# Senior JavaScript / TypeScript Developer Skill

Use this skill when developing, refactoring, or architecting JavaScript or TypeScript applications.

## Core Principles

- **Modern Syntax:** Always use modern ES6+ syntax (`let`/`const`, arrow functions, destructuring, optional chaining, nullish coalescing).
- **TypeScript First (If Applicable):** Leverage strong typing, interfaces, generics, and utility types to ensure type safety.
- **Functional & Object-Oriented Blends:** Use functional programming patterns (map, filter, reduce) for data transformation, and class-based patterns where state encapsulation makes sense.
- **Asynchronous Programming:** Prefer `async/await` over raw Promises or callbacks. Always handle rejections using `try/catch` or `.catch()`.

## Best Practices

1. **Immutability:** Treat data as immutable whenever possible to prevent side-effects. Use spread operators or libraries like Immer.
2. **Modularization:** Break down code into small, reusable ES Modules. Keep files focused on a single responsibility.
3. **Performance Optimization:** 
   - Debounce and throttle frequent events.
   - Use Web Workers for heavy computations.
   - Avoid memory leaks by cleaning up event listeners and intervals.
4. **Error Handling:** Implement robust error boundaries and global error handlers. Throw custom error classes for domain-specific issues.
5. **Testing:** Write testable code by injecting dependencies. Use Jest or Vitest for unit testing.

## Guardrails

- Do not pollute the global namespace.
- Avoid 'magic numbers' or hardcoded strings; extract them to constants.
- Avoid deeply nested callbacks (Callback Hell).
