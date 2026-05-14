---
name: qa-plan
description: Build a manual QA plan for a feature implemented via TDD. Triggered by "/qa-plan for issue <LINEAR-ID>". Identifies code changes, builds a checklist of manual QA steps, and posts it as a comment on the Linear issue.
---

# QA Plan Skill

## Trigger

`/qa-plan for issue <LINEAR-ISSUE-ID>`

(e.g. `/qa-plan for issue ENG-42`)

---

## Phase A — Understand What Changed

1. **Check context first.** Ask the user: "I have [summary of what's in context]. Does this cover the implementation, or should I pull the diff?" Only fetch if needed.
2. If not in context, find the changes:
   - Look for a PR linked to the issue: `gh pr list --search "<linear-issue-id>" --json number,headRefName,url`
   - If PR found, get the diff: `gh pr diff <pr-number>`
   - If no PR, find the branch: `git branch -a | grep feat/<linear-issue-id>` and diff against base: `git diff main...feat/<linear-issue-id>-*`
3. Read the Linear issue body for intended behavior and acceptance criteria using the Linear MCP (get issue by ID).
4. Briefly confirm with the user what you understand changed and what the feature does.

**CHECKPOINT: Wait for user confirmation.**

---

## Phase B — Build QA Plan

Write a QA plan as a checkbox list. Each item should be:
- A concrete, manually executable step (not vague like "test edge cases")
- A detailed setup section at the front which tells me explicitly (and if applicable) any infrastructure, environment variable etc. setup
- Focused on user-facing behavior, not code internals
- Ordered from happy path → edge cases → error cases

Keep it concise. A good QA item reads like: "Add an item to cart, proceed to checkout with an expired card — verify error message appears and cart is preserved."

---

## Phase C — Post to Linear

Post the QA plan as a comment on the Linear issue using the Linear MCP (create comment on issue).

Format:
```
## QA Plan

### Setup
// instructions to setup all components for the QA testing.

- [ ] QA Test 1
Steps to test:
// write steps here

- [ ] QA Test 2
Steps to test:
// write steps here

- [ ] ...
```

Tell the user the comment has been posted and they can check items off and leave replies directly on the Linear issue.
