---
name: prompt-deconstruction
description: Reverse-engineers complex, existing codebases into structured, sequential, step-by-step prompts. These generated prompts should guide other AI tools (like Cursor, Copilot, or specialized agents) to build the entire application from scratch with zero compilation errors and high fidelity.
---

# Prompt Engineering & Deconstruction

This skill specifies how to reverse-engineer full-featured code into clean, sequential, and highly directive prompt lists that other AI tools (e.g., Cursor, Claude, GPT) can read to rebuild the application or feature from scratch.

## Guidelines for Code Deconstruction & Prompt Generation

1. **Sequential Slicing (Step-by-Step Delivery)**:
   - Break down a large codebase into incremental slices (e.g., Setup -> Schema -> Service/Controller -> View/Widgets -> Integration).
   - Ensure each slice is fully functional or compiling before moving to the next.

2. **Structure of a Deconstructed Prompt**:
   - **Context & Goal**: Explain what is being built, the technology stack, and how it fits into the overall architecture.
   - **Detailed Requirements & File Structure**: Define the target folder structure and specific files to modify or create.
   - **Exact Code Details (Specs)**: Rather than giving vague instructions, describe the logic, validation rules, state flows, and exact dependencies.
   - **Verification Instructions**: Provide exact tests, lint rules, or terminal commands to run (e.g., `php artisan test`, `flutter analyze`) to verify compilation and execution.

3. **Eliminating Compilation Errors**:
   - Specify imports, dependency declarations, and version matches in `pubspec.yaml` or `composer.json` first.
   - Define model schemas and DTO contracts before drafting controller logic or UI templates.
   - Mandate precise error and edge case handling in the instructions so that the generated code is robust.
