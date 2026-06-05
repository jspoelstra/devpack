# AGENTS.md

Behavioral guidelines for AI coding agents (GitHub Copilot, Claude, Cursor, Aider, etc.) used in this repository.

These principles are intended to reduce common agent mistakes and keep changes safe, clear, and reviewable.

**Tradeoff:** These guidelines bias toward caution and quality over raw speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Do not assume. Surface tradeoffs and uncertainty early.**

Before implementing:
- State assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them.
- If a simpler approach exists, say so.
- If something is unclear, stop and ask a focused question.

## 2. Simplicity First

**Write the minimum code that solves the request. Nothing speculative.**

- No features beyond what was requested.
- No abstractions for single-use code.
- No configurability that was not asked for.
- No defensive branches for impossible scenarios.
- If the solution can be substantially smaller, simplify it.

## 3. Surgical Changes

**Touch only what is required.**

When editing existing code:
- Do not refactor unrelated areas.
- Do not reformat unrelated code.
- Match existing style and conventions.
- If unrelated issues are discovered, mention them without changing them.

When your change creates orphans:
- Remove imports, variables, or helpers made unused by your own edits.
- Do not remove pre-existing dead code unless requested.

## 4. Goal-Driven Execution

**Define success criteria and verify them.**

Turn requests into verifiable outcomes:
- Bug fix: reproduce with a test, then make it pass.
- New behavior: add/adjust tests to define expected behavior.
- Refactor: preserve behavior and verify before/after checks.

For multi-step tasks, use a brief plan:

```text
1. Step -> verify: check
2. Step -> verify: check
3. Step -> verify: check
```

## Working Agreement

This document applies repository-wide unless a more specific instruction file in a subdirectory overrides it.

Expected outcomes:
- Smaller, purpose-driven diffs.
- Fewer rewrites caused by over-engineering.
- Clarifying questions before implementation mistakes.
