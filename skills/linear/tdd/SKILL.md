---
name: tdd
description: Test-driven development with red-green-refactor loop. Use when user wants to build features or fix bugs using TDD, mentions "red-green-refactor", wants integration tests, or asks for test-first development.
---

# TDD Skill

## Philosophy

**Core principle**: Tests should verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't.

**Good tests** are integration-style: they exercise real code paths through public APIs. They describe _what_ the system does, not _how_ it does it. A good test reads like a specification - "user can checkout with valid cart" tells you exactly what capability exists. These tests survive refactors because they don't care about internal structure.

**Bad tests** are coupled to implementation. They mock internal collaborators, test private methods, try to test for specific passages in prompts or verify through external means (like querying a database directly instead of using the interface). The warning sign: your test breaks when you refactor, but behavior hasn't changed. If you rename an internal function and tests fail, those tests were testing implementation, not behavior.

See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

## Phase A — Orient

Two entry points — pick based on the user's request:

### A1. New feature/fix from an issue
Default for "build X", "implement Y", or a fresh issue.

1. Fetch the issue. Check blocking issues — stop if any are open.
2. **Ask before fetching linked TRDs/docs** — they may already be in context.
3. Examine relevant modules, tests, and public interfaces.
4. Brief the user: what's being asked, codebase findings, blockers, ambiguities.

### A2. Addressing PR review feedback
Use for "address the comments", "handle PR feedback", or references to review bots (cursor bugbot, Devin, etc.).

1. Identify the PR from the current branch; ask if ambiguous.
2. Fetch **unresolved** review comments only (inline + general, from user and bots).
3. Re-check the linked issue and **ask before fetching the TRD** if not in context.
4. Brief the user: group comments by concern, flag conflicts, and call out any worth pushing back on before acting.

**CHECKPOINT: Wait for user confirmation.**

---

## Phase B — Test Plan

Before writing any code:

- Confirm with user what interface changes are needed and which behaviors to test (prioritize)
- Identify opportunities for [deep modules](deep-modules.md) (small interface, deep implementation)
- Design interfaces for [testability](interface-design.md)

Propose an ordered list of behaviors to test — plain language, not code. Each describes what the system does from a caller's perspective. Order so each builds on the last; first behavior is the tracer bullet.

**CHECKPOINT: Wait for user approval.** User may reorder, cut, or add. This locks in what gets tested.

---

## Phase C — Red-Green Loop

1. Create feature branch: `feat/<LINEAR-ISSUE-ID>-<short-description>` (e.g. `feat/ENG-123-add-checkout`). The Linear issue ID in the branch name automatically links the PR to the issue once opened.
2. For each behavior:

**RED** — Write one test. Run suite. Confirm it fails. Show the test, failure output, and why it fails.

**GREEN** — Write minimal code to pass. Run full suite. Show the diff and explain what you added and why.

**MICRO-CHECKPOINT** — "Behavior 2/4 done. Next: [behavior]. Continue?"

### Rules
- One test at a time. Do not batch.
- Only enough code to pass the current test.
- Do not commit during this phase.

> **Environment note:** User uses `uv` as their Python environment manager. 

> **Testing note:** For running pytest specificially, run tests directly with `pytest ...` instead of `uv run pytest ...`.

### Escape hatch
If user says **"run the rest"** or **"finish it"**, batch remaining cycles. Show a summary of all tests, implementation, and full suite output at the end.

---

## Phase D — Refactor

- **Duplication** → Extract function/class
- **Long methods** → Break into private helpers (keep tests on public interface)
- **Shallow modules** → Combine or deepen
- **Feature envy** → Move logic to where data lives
- **Primitive obsession** → Introduce value objects
- **Existing code** the new code reveals as problematic


**CHECKPOINT: Wait for user approval.** Never refactor while RED.

---

## Phase E — Explain

Walk the user through the complete implementation:
- How the pieces fit together
- Key design decisions and trade-offs
- Anything surprising or worth noting for future work

Focus on the "why" and the big picture, not line-by-line narration.

---

## Phase F — Link PR to Linear

- Linear auto-links a PR when the issue ID appears in the **PR title**.
- **PR title**: must include the Linear issue ID somewhere, e.g. `[ENG-123] Add checkout flow`
- When creating the PR (`gh pr create`), set the title accordingly. If the PR already exists, check its title with `gh pr view --json title` and amend it with `gh pr edit --title "..."` if the issue ID is missing.
