# GitHub Copilot Instructions

> **Source**: Seeded from the `karpathy-principles` APM plugin (`plugins/karpathy-principles/templates/`).

This project uses the Karpathy-inspired guidelines defined in the root `AGENTS.md`.

**Always follow the four core principles from AGENTS.md:**

1. **Think Before Coding** — State assumptions, surface tradeoffs, ask when unclear.
2. **Simplicity First** — Minimum code that solves the problem. Ruthlessly avoid over-engineering.
3. **Surgical Changes** — Only touch what the request requires. Never do drive-by refactors.
4. **Goal-Driven Execution** — Define verifiable success criteria before coding. Loop until they pass.

Full details: see [AGENTS.md](../AGENTS.md) in the repository root.

Additional repo-specific notes for devpack (APM-based skill/plugin pack):
- Prefer changes that keep the APM dependency graph clean.
- Skills should be focused, with clear `name`, `description`, and step-by-step procedures.
- When editing skills, follow the SKILL.md frontmatter conventions used elsewhere in the pack.
- Test instructions mentally against "Would a senior engineer call this overcomplicated?"

