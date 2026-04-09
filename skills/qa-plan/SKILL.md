---
name: qa-plan
description: Build a manual QA plan for a feature implemented via TDD. Triggered by "/qa-plan for issue #X". Identifies code changes, builds a checklist of manual QA steps, and posts it as a comment on the GitHub issue.
---

# QA Plan Skill

## Trigger

`/qa-plan for issue #<number>`

---

## Phase A — Understand What Changed

1. **Check context first.** Ask the user: "I have [summary of what's in context]. Does this cover the implementation, or should I pull the diff?" Only fetch if needed.
2. If not in context, find the changes:
   - Look for a PR linked to the issue: `gh pr list --search "<issue-number>" --json number,headRefName,url`
   - If PR found, get the diff: `gh pr diff <pr-number>`
   - If no PR, find the branch: `git branch -a | grep feat/<issue-number>` and diff against base: `git diff main...feat/<issue-number>-*`
3. Read the issue body for intended behavior and acceptance criteria.
4. Briefly confirm with the user what you understand changed and what the feature does.

**CHECKPOINT: Wait for user confirmation.**

---

## Phase B — Build QA Plan

Write a QA plan as a checkbox list. Each item should be:
- A concrete, manually executable step (not vague like "test edge cases")
- Focused on user-facing behavior, not code internals
- Ordered from happy path → edge cases → error cases

Keep it concise. A good QA item reads like: "Add an item to cart, proceed to checkout with an expired card — verify error message appears and cart is preserved."

---

## Phase C — Post to GitHub

Post the QA plan as a comment on the issue using `gh issue comment <number> --body "<plan>"`.

Format:
```
## QA Plan

- [ ] Step 1
- [ ] Step 2
- [ ] ...
```

Tell the user the comment has been posted and they can check items off and leave replies directly on the issue.
