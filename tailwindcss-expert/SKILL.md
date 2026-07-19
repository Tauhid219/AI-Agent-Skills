---
name: tailwindcss-expert
description: Provides expert-level guidance for Tailwind CSS. Focuses on utility-first design patterns, responsive layouts, custom themes, arbitrary values, and framework integration.
---
# Tailwind CSS Expert Skill

Use this skill when styling applications with Tailwind CSS.

## Core Principles

- **Utility-First:** Build complex designs exclusively using utility classes. Resist the urge to write custom CSS unless absolutely necessary.
- **Responsive Design:** Utilize Tailwind's mobile-first responsive modifiers (`sm:`, `md:`, `lg:`, `xl:`, `2xl:`).
- **Component Extraction:** Extract heavily repeated class strings into reusable UI components (e.g., a React or Vue component) rather than using `@apply` in CSS, unless working in a templating environment without components.

## Best Practices

1. **Configuration:** Customize `tailwind.config.js` to extend colors, typography, and spacing to match the design system. Avoid overriding default keys unless intentional.
2. **Arbitrary Values:** Use arbitrary values (e.g., `top-[117px]`, `bg-[#bada55]`) for one-off magic numbers to avoid cluttering the config file.
3. **Dark Mode:** Leverage the `dark:` variant and configure dark mode strategy (class or media) in the configuration.
4. **Plugins:** Utilize official plugins (e.g., `@tailwindcss/forms`, `@tailwindcss/typography`) to style native elements efficiently.
5. **Class Sorting & Formatting:** Use Prettier with `prettier-plugin-tailwindcss` to automatically sort classes logically.

## Guardrails

- Avoid using `@apply` heavily. It breaks the utility-first paradigm and can lead to bloated CSS bundles.
- Do not concatenate class names dynamically in JS (e.g., `text-${color}-500`). Tailwind cannot purge these. Use full class names dynamically or a library like `clsx` or `tailwind-merge`.
