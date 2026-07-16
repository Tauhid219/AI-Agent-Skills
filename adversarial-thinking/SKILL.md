---
name: adversarial-thinking
description: Enforces an adversarial "code breaker" mindset to prevent happy-path-only development. Requires the generation of an Edge Case Matrix (handling Null, Empty, Negative, Duplicate, and Unexpected types) before writing any code, ensuring Boundary Value Analysis and edge cases are addressed upfront.
---

# Adversarial Thinking & Edge Case Matrix

Do not just write code for the "happy path". Adopt an adversarial, "code breaker" mindset. Act as a strict quality assurance tester whose goal is to figure out how to crash the system.

## Core Capabilities

1. **Negative Testing Mindset**: Before implementing any function, ask yourself: "How can I break this code?"
2. **Boundary Value Analysis (BVA)**: Analyze and handle extreme limits (e.g., massive numbers, empty strings, negative values, out-of-bounds dates).

## Mandatory Workflow Step: Edge Case Matrix

**CRITICAL INSTRUCTION**: Before writing any functional code, you MUST generate an **Edge Case Matrix**. 

When presented with a task, analyze how the code will behave under the following scenarios:
- **Null / None / Undefined** inputs
- **Empty Inputs** (e.g., `[]`, `""`, `{}`)
- **Extreme Numbers** (e.g., Negative values, Zero, Overflow/Large numbers)
- **Duplicate Data** (e.g., identical submissions, non-unique keys)
- **Unexpected Data Types** (e.g., passing a string when an integer is expected, passing objects instead of arrays)
- **Context-Specific / Business Logic Extremes** (e.g., dates in the future, logically impossible ranges, invalid string formats like wrong email structures)

## Integration with Other Skills

This workflow integrates with existing skills:
1. **Edge Case Analysis**: Generate the Edge Case Matrix using this skill.
2. **Defensive Coding**: Apply input validation and robust try-catch exception handling (`production-resilience` skill).
3. **Test Case Generation**: Write automated unit tests covering all matrix scenarios (`tdd` skill).
4. **Self-Correction Loop**: Execute tests and fix implementation failures autonomously.
