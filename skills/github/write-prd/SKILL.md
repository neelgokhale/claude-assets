---
name: write-prd
description: Create a product-focused PRD covering personas, user stories, and a detailed end-to-end user flow, then file it as a GitHub issue (with "prd" label). Use when the user wants to write a PRD, draft a product requirements document, or document the product surface of a feature. Supports being run after a "grill-me" design session.
---

This skill writes a **product-focused** PRD. It describes who the user is, what they want to do, and what they should see and feel at each step. It does **not** describe modules, schemas, APIs, or other implementation details — those belong in a TRD (`write-trd`).

## 1. Identify the trigger path

Ask the user (or infer from context) which path applies:

- **Path A — Post-grill-me**: A deep design-thinking session just happened. Use that conversation as the primary source. Skim it for user pain, decisions, and feature scope, then fill the gaps via interview.
- **Path B — From scratch**: No prior context. Run a product-focused interview yourself (problem, users, goals, flows) before drafting.

## 2. Interview to fill product gaps

Ask the user, one question at a time, until the product surface is fully resolved. Focus areas:

- **Users & personas**: Who exactly is this for? What's their context when they hit this feature?
- **Goals & non-goals**: What does success look like for the user *and* the business? What are we explicitly **not** trying to do?
- **End-to-end flow**: Walk the entire happy path step-by-step. At each step, capture: what the user does, what they see, what they expect to happen, and any state the system holds for them.
- **Edge cases & alternate flows**: What happens on empty state, error, slow network, partial input, permission denied, etc.? Describe these from the user's perspective, not the system's.
- **Success metrics**: How will we know this worked? (Adoption, conversion, retention, support-ticket reduction, etc.)

For each question, recommend an answer based on what's already known. If a question is answerable from a grill-me transcript or the codebase, answer it yourself instead of asking.

## 3. Confirm scope, then draft

Once the product picture is complete, summarize the key product decisions back to the user in 5–10 bullet points and confirm before drafting. Then write the PRD using the template below.

## 4. File the PRD in GitHub

Create a GitHub issue using the `gh` CLI:

- **Title**: short, product-flavored title for the feature (not a technical name)
- **Body**: the full PRD content using the template below
- **Label**: "prd" (create the label if it does not already exist in the repo with `gh label create prd`)

Use: `gh issue create --title "..." --body "..." --label "prd"`

After creating the issue, share the GitHub issue URL with the user.

<prd-template>

## Problem Statement

The user-facing problem, told from the user's perspective. Include the context in which the user hits this problem and why current alternatives are insufficient.

## Goals & Non-Goals

**Goals** (what success looks like):
- Goal 1 — stated as a user or business outcome, not a feature
- ...

**Non-Goals** (what we are explicitly NOT trying to do in this scope):
- Non-goal 1
- ...

## User Stories

A LONG, numbered list of user stories. Each in the format:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

This list should be extensive and cover every product surface touched by the feature.

## End-to-End User Flow

The detailed happy path, step-by-step. For each step, capture:

- **User action**: what the user does (clicks, types, navigates, etc.)
- **What they see**: the visible UI state, copy, key elements
- **What they expect**: the user's mental model of what should happen next
- **System response**: what actually happens (without going into implementation detail)

<flow-step-example>
**Step 3 — Confirm transfer amount**
- User action: Enters $250 in the amount field and taps "Continue"
- What they see: Amount field shows formatted "$250.00", "Continue" button activates, brief inline validation if amount exceeds available balance
- What they expect: To be taken to a confirmation screen showing the recipient, amount, and fee before any money moves
- System response: Navigates to the confirmation screen with all transfer details summarized and an explicit "Send" CTA
</flow-step-example>

Number the steps. Be exhaustive — if the happy path has 12 steps, write all 12.

## Alternate Flows & Edge Cases

For each notable deviation from the happy path, describe what the user sees and does. Cover at minimum:

- Empty state (first-time user, no data yet)
- Loading / slow network state
- Error states (validation, server, permission, auth)
- Partial / interrupted flows (user backgrounds the app, loses connection mid-flow)
- Recovery paths (how does the user get back on track?)

## UX Details

Anything that shapes the feel of the feature:

- Key copy (headlines, button labels, error messages) where wording matters
- Notable interaction patterns (animations, transitions, haptics, sounds)
- Accessibility requirements
- Localization considerations
- Visual hierarchy / what should be most prominent

## Out of Scope

What this PRD explicitly does **not** cover. Reference what's deferred to a follow-up.

## Open Questions

Unresolved product questions that need answers before or during implementation. Each should have an owner or a path to resolution.

## Further Notes

Any additional context, prior art, competitive references, or background that informed the decisions above.

</prd-template>
