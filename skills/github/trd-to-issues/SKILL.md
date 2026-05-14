---
name: trd-to-issues
description: Break a TRD GitHub issue into independently-grabbable sub-issues using tracer-bullet vertical slices. Use when user wants to convert a TRD to issues, create implementation tickets, or break down a TRD into work items.
---
# TRD to Issues
Break a TRD GitHub issue into independently-grabbable sub-issues using vertical slices (tracer bullets).

## Process
### 1. Locate the TRD issue
Ask the user for the GitHub issue number (or URL) of the TRD issue (created by the `write-trd` skill, labelled "trd").
If the TRD content is not already in your context window, fetch it using `gh issue view <number>` to retrieve its body.

### 2. Explore the codebase (optional)
If you have not already explored the codebase, do so to understand the current state of the code.

### 3. Draft vertical slices
Break the TRD into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer. Follow these rules:

- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- The question of many thin slices vs. few thick ones should be judged based on how big the scope of the feature is
- All surfaces from the TRD must be accounted for across the full set of slices — don't leave gaps
- No two slices should significantly overlap in what they build; if overlap is detected, merge those slices. If a slice is difficult to define cleanly, stop and ask the user for guidance rather than guessing
- **QA testability:** A completed slice is demoable or verifiable on its own. Each slice should leave the system in a state where at least some part of the feature can be manually QA'd end-to-end. Prioritize slice boundaries that unlock real QA checkpoints; note what is QA-able at the end of each slice

### 4. Quiz the user
Present the proposed breakdown as a numbered list. For each slice, show:
- **Title**: short descriptive name
- **Blocked by**: which other slices (if any) must complete first
- **QA checkpoint**: what can be manually tested after this slice lands
- **User stories covered**: which user stories from the TRD this addresses

Ask the user:
- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?

Iterate until the user approves the breakdown.

### 5. Create the GitHub sub-issues
For each approved slice, create a GitHub issue using the `gh` CLI. Create issues in dependency order (blockers first) so you can reference real GitHub issue numbers in the "Blocked by" field.

**Creating issues:** Use `gh issue create --title "..." --body "..." --label "trd-slice"` to create each issue.

**Setting up as sub-issues of the TRD:** After creating each issue, add it as a sub-issue of the TRD issue using `gh issue develop` or by editing the TRD issue body to include a task list referencing the new issues (e.g., `- [ ] #123`).

**Determining the TRD issue number:** It comes from Step 1. If the user hasn't provided it and the TRD is only in context as a document (not yet filed as a GitHub issue), ask before proceeding.

Use the issue description template below.

<issue-template>
## Parent TRD
#[GitHub issue number of the TRD issue]

## What to build
A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation. Reference specific sections of the parent TRD rather than duplicating content.

## Acceptance criteria
- [ ] Criterion 1
- [ ] ...

## QA checkpoint
What can be manually tested end-to-end once this slice is merged.

## Blocked by
- Blocked by #[GitHub issue number] (if any)
Or "None - can start immediately" if no blockers.

## User stories addressed
Reference by number from the parent TRD:
- User story 3
- ...
</issue-template>

Do NOT modify the parent TRD issue (other than adding the sub-issue task list).
