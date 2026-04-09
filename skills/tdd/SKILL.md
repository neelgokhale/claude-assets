---
name: tdd
description: Test-driven development from GitHub issue to PR. Two triggers. "implement issue #X with /tdd" for building features. "resolve qa comments on issue #X" for fixing QA feedback.
---

# TDD Skill

## Triggers

**Build:** `implement issue #<number> with /tdd`
→ Start at Phase A.

**QA Resolution:** `resolve qa comments on issue #<number>`
→ Start at Phase Q.

---

## Phase A — Orient

1. Fetch the GitHub issue. Read the full body.
2. Check blocking issues — if any are open, stop and tell the user.
3. **Before loading any linked PRD or referenced documents, ask the user if they're already in context.** Only fetch what's missing.
4. Examine relevant codebase: modules, existing tests, public interfaces that will be touched.
5. Brief the user: what the issue asks for (in your own words), relevant codebase findings, blocker status, and any ambiguities.

**CHECKPOINT: Wait for user confirmation.**

---

## Phase B — Test Plan

1. Propose public interface changes (if any).
2. Propose an ordered list of behaviors to test — plain language, not code. Each describes what the system does from a caller's perspective. Order so each builds on the last; first behavior is the tracer bullet.

Example:
```
Interface: CartService.checkout(cartId, paymentMethod) → OrderConfirmation

Behaviors:
1. Valid cart and payment → confirmation with order ID
2. Empty cart → validation error
3. Expired payment → payment error
4. Successful checkout decrements inventory
```

**CHECKPOINT: Wait for user approval.** User may reorder, cut, or add. This locks in what gets tested.

---

## Phase C — Red-Green Loop

1. Create feature branch: `feat/<issue-number>-<short-description>`
2. For each behavior:

**RED** — Write one test. Run suite. Confirm it fails. Show the test, failure output, and why it fails.

**GREEN** — Write minimal code to pass. Run full suite. Show the diff and explain what you added and why.

**MICRO-CHECKPOINT** — "Behavior 2/4 done. Next: [behavior]. Continue?"

### Rules
- One test at a time. Do not batch.
- Only enough code to pass the current test.
- Do not commit during this phase.

### Escape hatch
If user says **"run the rest"** or **"finish it"**, batch remaining cycles. Show a summary of all tests, implementation, and full suite output at the end.

---

## Phase D — Refactor

Once all behaviors are green:

1. Look for duplication, naming improvements, module depth, SOLID where natural.
2. Each refactor: change, run suite, confirm green.
3. Show what you changed and why.

**CHECKPOINT: Wait for user approval.** Never refactor while RED.

---

## Phase E — Explain

Walk the user through the complete implementation:
- How the pieces fit together
- Key design decisions and trade-offs
- Anything surprising or worth noting for future work

Focus on the "why" and the big picture, not line-by-line narration.

---

## Phase F — Commit & PR

1. Commit all work in a single commit:
   ```
   feat(#<issue-number>): <short description>

   - <behavior 1>
   - <behavior 2>
   - ...
   ```
   For larger features, ask if user prefers two commits (implementation + refactors).
2. Push and open PR: link the issue, describe behaviors tested, note design decisions.

---

## Phase Q — QA Resolution

Triggered by: `resolve qa comments on issue #<number>`

1. Fetch all comments on the issue: `gh issue view <number> --json comments --jq '.comments[].body'`
2. Summarize the QA feedback to the user as a concise list — what was found, what needs fixing.
3. **CHECKPOINT: Wait for user to confirm which items to address.** User may deprioritize or clarify.
4. For each item to fix, follow the same red-green discipline:
   - Write a test that reproduces the reported problem (RED, confirm it fails).
   - Fix it (GREEN, confirm all tests pass).
   - Show the user what you did.
5. Once all items are addressed, run refactor pass (Phase D), explain (Phase E), then commit and push to the existing feature branch. Update the PR.

---

## Principles

**Test behavior, not implementation.** Tests use public interfaces. If a test breaks on refactor but behavior hasn't changed, the test was wrong.

**Vertical slices, not horizontal.** One test → one implementation → repeat. Never write all tests first.

**You can't test everything.** Phase B is where prioritization happens. User decides what matters.

### Per-cycle check
- Test describes behavior, not implementation
- Test uses public interface only
- Test survives internal refactor
- Code is minimal for this test
- No speculative features