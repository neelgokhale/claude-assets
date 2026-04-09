# My workflow on claude code to ship small/large features

This is the workflow I typically follow to ship small to large features into a project or instantiate an empty codebase into a solution. It relies heavily on the typical chat interaction pattern on Claude Code \+ some specifically designed skills. I also use Github as a way to store intermediate specs, PRDs, issues and QA/testing logs throughout the end-to-end development and release cycle of the feature. 

## Key Skills and Steps

### 1\. Feature Definition and Architecture Planning (`/grill-me`)

An intensive interrogation session with Claude serving as a copilot architect. The goal is to move from a high-level core idea to a fully fleshed-out feature specification and preliminary architecture design.

**Skill:** `/grill-me`

**Process:**

* Present the core idea and ask Claude to `/grill-me` on all potential ambiguities, edge cases, data structures, dependencies, and architectural choices.  
* Engage in a rapid back-and-forth, which can range from 10 to 30 turns of questioning.

**Output:** A detailed feature specification, including key technical decisions, a high-level architecture (often described in text), and a list of major components/modules. This specification is often captured and stored in a Github issue for later reference.

## 2\. PRD Drafting and Validation (/write-prd)

After the initial **`/grill-me`** session, the raw output is refined into a formal PRD.

**Skill:** /write-prd

**Process:**

* Ask Claude to refine the detailed feature specification from the previous step into a structured PRD draft, adhering to key structural guidelines (e.g., sections for Goals, Non-Goals, User Stories, Technical Spec, Success Metrics).  
* Review and validate the content with Claude, focusing on clarity, completeness, and alignment with the initial core idea.  
* Once validated, push the final PRD content as a new Github Issue (the "master" issue) in the repository.

**Output:** A validated, structured PRD documented as a master Github Issue, serving as the single source of truth for the feature.

## 3\. Breaking Down the PRD into Slices (/prd-to-issues)

The master PRD is then decomposed into smaller, actionable, and vertically-sliced implementation tasks.

**Skill:** /prd-to-issues

**Process:**

* Revisit the master PRD.  
* Ask Claude to identify well-scoped "vertical slices" of implementation. These slices should aim to deliver a small, end-to-end piece of functionality to help identify and test "unknown unknowns" early across the feature's surface area (touching backend, db, frontend etc,)  
* For each vertical slice, create a new, individual Github Issue and link it back to the master PRD issue.

**Output:** A set of smaller, interconnected Github Issues, each representing a manageable implementation slice ready for development.

# 4\. Test-Driven Development (TDD) of Slices (/tdd)

Each slice issue is developed following a strict Test-Driven Development (TDD) approach within Claude Code.

**Skill:** /tdd

**Process:**

* Reference a single Github Issue (a vertical slice) in a new Claude Code session.  
* Instruct Claude to develop the slice using the TDD RED-GREEN pattern: write failing tests (RED), write minimum code to pass tests (GREEN), and refactor.  
* Ensure the development session is concluded with a full run of all unit tests to confirm success.  
* Push the final slice implementation code as a PR to the repository, describing the implementation and linking it to the corresponding Github Issue.

**Output:** A completed feature slice, pushed as a validated PR with passing unit tests, ready for integration/deployment.

## 5\. QA Planning and Execution

Once a slice or the full feature has been developed and deployed to a testing environment, a comprehensive QA plan is created and executed.

**Skill:** /qa-plan

**Process:**

* Build a detailed QA plan and test cases based on the PRD and the developed slices to comprehensively test the feature's functionality and edge cases.  
* QA plan and execution results are recorded directly as comments within the corresponding Github Issue.  
* The user engages with the QA plan, manually testing and leaving feedback, bug reports, and necessary revisions directly as comments on the issue.

**Output:** A documented QA log, including user feedback and new action items (comments) left on the Github Issue.

## 6\. Cycle Repetition and Iteration

The workflow repeats until the feature is complete and stable.

**Skill:** /tdd (leveraging previous context)

**Process:**

* For any new feedback, bug reports, or necessary revisions left as comments on Issue \#X, instruct Claude to re-engage with TDD to address them.  
* The instruction is: "Can you **/tdd all the comments left on issue \#X**".

**Output:** A refined feature, thoroughly tested and validated against the initial PRD.