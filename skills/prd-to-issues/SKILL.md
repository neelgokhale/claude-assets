---
name: prd-to-issues
description: Break a PRD into independently-grabbable GitHub issues using tracer-bullet vertical slices. Use when user wants to convert a PRD to issues, create implementation tickets, or break down a PRD into work items.
---

# PRD to Issues

Break a PRD into independently-grabbable GitHub issues using vertical slices (tracer bullets).

## Process

### 1. Locate the PRD

Ask the user for the PRD GitHub issue number (or URL).
If the PRD is not already in your context window, fetch it with `gh issue view <number>` (with comments).

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code.

### 3. Draft vertical slices

Break the PRD into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

Slices may be 'HITL' or 'AFK'. HITL slices require human interaction, such as an architectural decision or a design review. AFK slices can be implemented and merged without human interaction. Prefer AFK over HITL where possible.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories from the PRD this addresses

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the correct slices marked as HITL and AFK?

Iterate until the user approves the breakdown.

### 5. Create the GitHub issues

For each approved slice, create a GitHub issue using `gh issue create`. Create issues in dependency order (blockers first) so you can reference real issue numbers in the "Blocked by" field.

**Linking to parent PRD:** After creating each issue, link it to the PRD as a sub-issue:

```
gh sub-issue add <prd-issue-number> <child-issue-number>
```

**Determining the PRD issue number:** The PRD issue may come from the current context (if the user just created it) or from a previous session. Either way, the user provides the issue number in Step 1 — use that number for linking. If the user doesn't provide a number and the PRD is only in context as a document (not yet filed as an issue), ask before proceeding.

Use the issue body template below.

<issue-template>
## Parent PRD
#<prd-issue-number>

## What to build
A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation. Reference specific sections of the parent PRD rather than duplicating content.

## Acceptance criteria
- [ ] Criterion 1
- [ ] ...

## Blocked by
- Blocked by #<issue-number> (if any)
Or "None - can start immediately" if no blockers.

## User stories addressed
Reference by number from the parent PRD:
- User story 3
- ...
</issue-template>

Do NOT close or modify the parent PRD issue.
