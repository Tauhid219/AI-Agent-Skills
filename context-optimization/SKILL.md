---
name: context-optimization
description: Optimizes context, prompt layouts, and repository structure for LLM/AI understanding. Minimizes context window bloating by focusing on token efficiency, clear instructions, system states, and structured metadata.
---

# Context Optimization & Clarity

This skill ensures that all codebase context, prompt structures, and file lists provided to an AI are highly optimized, token-efficient, and easy to parse, maximizing response accuracy and minimizing errors.

## Context Presentation Guidelines

1. **Information Density**:
   - Provide minimal but sufficient code context. Avoid sending entire repositories or giant files if only a few lines/functions are relevant.
   - Use folder tree structures and summaries to orient the AI before exposing detailed files.

2. **Clean File Formatting**:
   - Structure file references with absolute/relative paths and clear line number ranges (e.g., `app/Http/Controllers/UserController.php#L20-L50`).
   - Use clean Markdown syntax, fenced code blocks with language identifiers, and descriptive file headers.

3. **Pruning Irrelevant Code**:
   - Strip unrelated helper classes, vendor dependencies, compiled folders (e.g., `build/`, `vendor/`, `.dart_tool/`), and binary assets from prompts.
   - Focus on data contracts, interface declarations, database migrations, and active component interactions.

4. **Defining System States**:
   - Make the "Current State" vs. "Target State" explicit to prevent the AI from generating redundant solutions or re-implementing existing features.
