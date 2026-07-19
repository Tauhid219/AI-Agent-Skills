---
name: senior-vue-developer
description: Provides senior-level Vue.js guidance. Focuses on Vue 3 Composition API, reactivity, state management (Pinia), routing, and Nuxt.js best practices.
---
# Senior Vue Developer Skill

Use this skill when developing or refactoring Vue.js applications.

## Core Principles

- **Vue 3 Composition API:** Prefer `<script setup>` and the Composition API over the Options API for better logic reuse and TypeScript support.
- **Reactivity:** Understand the difference between `ref` and `reactive`. Use `ref` for primitives and mostly everything for consistency, use `reactive` for grouped state objects.
- **State Management:** Use **Pinia** instead of Vuex for global state. Keep Pinia stores modular.

## Best Practices

1. **Composables:** Extract reusable logic into composables (e.g., `useUser()`, `useMouse()`). Follow the convention of returning reactive state and functions.
2. **Component Design:** Keep components small and focused. Use slots and scoped slots for highly flexible layout components.
3. **Performance Optimization:**
   - Use `v-show` instead of `v-if` for frequently toggled elements.
   - Use `defineAsyncComponent` for lazy loading routes and heavy components.
   - Be mindful of reactivity tracking; avoid making large, complex objects deeply reactive if unnecessary (`shallowRef`).
4. **Lifecycle Hooks:** Use `onMounted`, `onUnmounted`, etc. properly. Always clean up third-party library instances or event listeners.
5. **Nuxt.js Context:** When in Nuxt, leverage auto-imports, Server-Side Rendering (SSR), and hybrid rendering configurations appropriately.

## Guardrails

- Never mutate props directly; always emit an event to the parent.
- Ensure stable and unique `:key` bindings in `v-for` loops.
- Do not mix Options API and Composition API in the same component unnecessarily.
