---
name: flutter-developer
description: Provides guidance for Flutter application development, including UI implementation, widget slicing, state management (Bloc, Riverpod, Provider), routing, asset management, API integration, and responsive design. Use when working on Flutter or Dart projects.
---

# Flutter Developer

This skill provides step-by-step guidance on how to develop, architect, and integrate UI designs and features into a Flutter application. It ensures consistent patterns for state management, widget composition, asset organization, and API consumption.

## Workflow

### 1. Project Analysis & Discovery
Before writing code, inspect the codebase to understand its architectural patterns and dependencies:
- **State Management**: Open `pubspec.yaml` to identify the state management library in use (e.g., `flutter_bloc`, `flutter_riverpod`, `provider`, `get`, or simple stateful widgets).
- **Navigation & Routing**: Determine if the project uses Flutter's native Navigator, `go_router`, `auto_route`, or simple named routes.
- **Folder Structure**: Identify if the project is organized by feature (feature-first) or by layer (layer-first):
  - **Feature-first**: `lib/features/auth/{presentation, domain, data}/`
  - **Layer-first**: `lib/{views, models, controllers, services, widgets}/`
- **Network & Serialization**: Check for networking libraries (`dio`, `http`) and code generators (`freezed`, `json_serializable`).

### 2. Assets & Configuration
- **Asset Registration**: Place new assets (images, icons, fonts, animations) in a dedicated folder (e.g., `assets/images/`, `assets/icons/`).
- **Pubspec updates**: Declare the assets correctly in `pubspec.yaml`:
  ```yaml
  flutter:
    assets:
      - assets/images/
      - assets/icons/logo.png
  ```
- **Fonts**: Register custom typography in `pubspec.yaml` and configure the theme in your `ThemeData`.

### 3. Widget Slicing & UI Composition
When implementing a new design or layout:
- **Responsive Layouts**: Use `LayoutBuilder`, `MediaQuery`, or packages like `flutter_screenutil` to adapt to different screen dimensions (mobile, tablet, desktop).
- **Slicing Widgets**: Break down complex UI screens into modular components:
  - Put reusable components in a global `widgets/` folder (e.g., custom buttons, input fields).
  - Put screen-specific widgets in a local subfolder next to the page/view.
- **Performance Optimization**:
  - Prefer `const` constructors wherever possible to avoid unnecessary rebuilds.
  - Use `ListView.builder` or `GridView.builder` instead of non-builder equivalents for dynamic/large datasets.
- **Theme Consistency**: Utilize `Theme.of(context).colorScheme` and `Theme.of(context).textTheme` rather than hardcoding colors and font styles.

### 4. State Management Integration
Ensure UI elements are separated from business logic:
- **Separation of Concerns**: Keep views thin. Business logic should reside in Blocs, Cubits, Notifiers, or Controllers.
- **Events & States**:
  - *Bloc/Cubit*: Map user interactions to Bloc events or Cubit methods, and use `BlocBuilder`/`BlocConsumer` to render states.
  - *Riverpod*: Use `ConsumerWidget` or `ConsumerStatefulWidget` and access states with `ref.watch()`.
  - *Provider*: Use `Consumer` or `context.watch<MyNotifier>()` for reactive changes.

### 5. API & Data Integration
- **Serialization**: Create Data Transfer Objects (DTOs) or Models. If using `json_serializable` or `freezed`, run `dart run build_runner build --delete-conflicting-outputs` to generate helper files.
- **Service Layer**: Implement service classes/repositories to handle API requests using `Dio` or `Http`.
- **Error Handling**: Gracefully catch network errors and display user-friendly messages via `SnackBar`s, dialogs, or dedicated error states in the UI.

### 6. Verification & Quality Checks
- Run compiler checks: Make sure there are no Dart analyzer warnings or syntax errors.
- **Command Line Check**: Run `flutter analyze` and `flutter test` (if unit or widget tests are configured).
- **Responsive Check**: Test the app layouts across multiple simulated resolutions (e.g., small screens, tablets).
