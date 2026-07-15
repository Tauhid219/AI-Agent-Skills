---
name: laravel-ui-integrator
description: Converts, integrates, and sets up HTML, Bootstrap, or Tailwind templates into a Laravel UI stack, supporting Blade, React, Vue, or Inertia.js. Use when the user requests converting static templates into Laravel views, layouts, or components.
---

# Laravel UI Integrator

This skill provides step-by-step guidance on how to parse, slice, and integrate HTML, Bootstrap, or Tailwind templates into an existing Laravel application, adapting to the target frontend technology stack (Blade, React, Vue, Inertia, or Livewire).

## Workflow

### 1. Project Stack Identification & Discovery
Before moving any code, examine the Laravel environment to understand how styles and assets are managed:
- **Build Tools**: Check `package.json` and root configuration files to see if the project uses **Vite** (`vite.config.js`) or **Laravel Mix** (`webpack.mix.js`).
- **Frontend Stack**: 
  - Look in `resources/views` for traditional Blade-only structures.
  - Look in `resources/js/` for Vue (`Vue.createApp`, Inertia components) or React (`createRoot`, Inertia components) setups.
  - Check `composer.json` for **Livewire**, **Inertia**, or **Breeze/Jetstream** packages.
- **CSS Framework**: Confirm if the template and project match (Tailwind vs. Bootstrap). If they differ, ensure necessary Tailwind configurations (`tailwind.config.js`) or Bootstrap packages are installed.

### 2. Asset Organization & Migration
Prepare the template's static assets for Laravel:
- **Public Assets**: Place images, fonts, and vendor-specific files that do not need compilation into the `public/` directory (e.g., `public/images/`, `public/fonts/`).
- **Styles & Scripts**: 
  - For compiled setups: Move standard CSS/JS files into `resources/css/` and `resources/js/`.
  - For simple structures: Move them to `public/css/` and `public/js/`.

### 3. Slicing & Creating UI Components

#### A. Traditional Blade Views
1. **Master Layout**: Create or modify a master layout file (e.g., `resources/views/layouts/app.blade.php`).
   - Extract the common `<head>`, header, footer, and sidebar.
   - Use `@yield('content')` or `<main> {{ $slot }} </main>` for dynamic content injection.
2. **Asset Helpers**: Replace all static asset paths with dynamic helpers:
   - Use `{{ asset('images/logo.png') }}` for public folder assets.
   - Use `@vite(['resources/css/app.css', 'resources/js/app.js'])` for Vite-compiled assets.
3. **Subelements / Partials**: Extract repeatable sections (e.g., navigation, alert boxes, sidebar items) to components/partials (`resources/views/components/` or `resources/views/partials/`).
4. **Individual Views**: Create view files extending the layout:
   ```blade
   @extends('layouts.app')

   @section('content')
       <!-- Page-specific content here -->
   @endsection
   ```

#### B. Inertia.js with React / Vue
1. **Inertia Layouts**: Create a layout component (e.g., `Layout.jsx` or `Layout.vue`) containing common elements (Header, Sidebar, Footer) and rendering `{children}` (React) or `<slot />` (Vue).
2. **Page Components**: Convert template pages into React/Vue components (e.g., `Pages/Dashboard.jsx` or `Pages/Dashboard.vue`).
3. **Asset Handling**: 
   - Import static assets relative to JS files or use static URL paths depending on the bundler configuration.
   - Convert standard `href="..."` links to Inertia `<Link href="...">` tags to prevent full page reloads.
4. **Class Name Conversion (for React)**: Convert all `class="..."` properties in HTML to `className="..."` and self-close non-paired tags (like `<img>`, `<input>`, `<hr>`).

### 4. Route Hookup & Verification
- Match views or Inertia components to relevant controller/route return statements.
- Run frontend build commands to test compilation:
  - `npm run dev` (for development)
  - `npm run build` (to ensure production asset compilation is error-free)
- Verify layout styling, responsive behavior, console errors, and network paths for missing assets.
