---
name: senior-react-developer
description: Provides senior-level React.js guidance. Focuses on React 18+ features, hooks, component architecture, state management, performance tuning, and Next.js integration.
---
# Senior React Developer Skill

Use this skill when building or refactoring React applications.

## Core Principles

- **Functional Components:** Exclusively use functional components and Hooks. Do not use class components.
- **Hook Rules:** Strictly adhere to the Rules of Hooks. Never call hooks conditionally.
- **Component Architecture:** Divide components into "Smart" (Container) and "Dumb" (Presentational) components.
- **State Management:** Keep state as local as possible. Use Context API for theme/auth, and libraries like Zustand or Redux Toolkit for complex global state.

## Best Practices

1. **Performance Optimization:**
   - Use `React.memo`, `useMemo`, and `useCallback` judiciously to prevent unnecessary re-renders. Do not over-optimize prematurely.
   - Implement code splitting and lazy loading (`React.lazy`, `Suspense`).
2. **Custom Hooks:** Extract reusable business logic and side effects into custom hooks (e.g., `useFetch`, `useAuth`).
3. **Side Effects:** Keep `useEffect` dependencies accurate. Clean up subscriptions and timers in the cleanup function.
4. **Styling:** Use standard CSS modules, styled-components, or utility frameworks like Tailwind CSS properly. Avoid inline styles for complex layouts.
5. **Next.js Integration:** When using Next.js, leverage Server Components (RSC) and proper data fetching strategies (SSR, SSG) based on requirements.

## Guardrails

- Do not mutate state directly. Always use the setter function provided by `useState` or `useReducer`.
- Avoid prop drilling by utilizing Composition (passing `children`) or Context.
- Ensure all lists have unique and stable `key` props (avoid using array indices).
