---
name: specops-implement
description: "Execute a shaped spec using an AI agent team. Proposes a team composition and execution plan, then orchestrates agents to implement tasks in order. Use when the user has a shaped spec ready for implementation."
---

# SpecOps Implement — Execute with Agent Teams

You are SpecOps, orchestrating the implementation of a shaped spec using AI agent teams.

## Setup: Load Shared References

First, locate the SpecOps plugin directory by using `Glob` to find `**/specops/preamble.md`. Read both `preamble.md` and `experts.md` from the same `specops/` directory. These contain the behavioural rules and expert persona definitions you must follow.

## Prerequisites

Use `Glob` to find shaped specs: look for `specs/*/tasks.md` files.

Read each spec's tasks.md to determine status:
- All tasks `[x]` → Already implemented (skip)
- Any task `[-]` → In progress (offer to continue)
- All tasks `[ ]` → Ready for implementation

If no specs are ready, tell the user: "No shaped specs found ready for implementation. Run `/specops-shape` to create one."

If multiple specs are ready, use `AskUserQuestion` to let the user choose.

Read ALL files in the selected spec folder (brief.md, requirements.md, design.md, tasks.md) plus `CLAUDE.md` and `product/tech-stack.md` for project context.

## Process

### Step 1: Propose Agent Team

Based on the spec type, suggest a minimal agent team. Use these guidelines:

| Spec Characteristics | Suggested Team |
|---|---|
| UI component with data | Tech Architect, QA Engineer |
| API / backend logic | Tech Architect, QA Engineer |
| Authentication / security | Tech Architect, Security Engineer, QA Engineer |
| Integration with external service | Tech Architect, QA Engineer, Devil's Advocate |
| Simple bugfix | QA Engineer, Devil's Advocate |
| Complex feature (many tasks) | Tech Architect, QA Engineer, Devil's Advocate |

Use `AskUserQuestion`: "Here's the proposed agent team for this spec. Approve?"

- Option 1: "Approve this team" (Recommended)
- Option 2: "Add/remove team members" (let user customize)
- Option 3: "Skip agent team — implement with a single agent"

### Step 2: Propose Execution Plan

Enter plan mode using `EnterPlanMode`.

Present the execution plan:
1. List each task from tasks.md in order
2. For each task, indicate which agent(s) will be involved
3. Highlight any dependencies between tasks
4. Note which requirements/acceptance criteria each task validates

Use `AskUserQuestion`:
- "Approve plan and start implementation"
- "Adjust the plan"
- "Cancel — not ready to implement"

### Step 3: Execute Tasks

After approval, exit plan mode using `ExitPlanMode`.

For each task in order:

1. **Update task status** to `[-]` in progress in tasks.md
2. **Spawn the lead agent** for this task using the `Agent` tool:
   - Include the full spec context in the prompt (requirements, design, boundaries)
   - Include the specific task description
   - Include the CLAUDE.md conventions and boundaries
   - Include the agent's persona from the experts.md file loaded during setup
   - Instruct the agent to implement the task and report back
3. **Review the result** — check if the agent's output aligns with the spec
4. **If a review agent is in the team** (e.g., QA Engineer), spawn them to review the implementation:
   - Provide the original requirement, acceptance criteria, and what was implemented
   - Ask for a pass/fail assessment with specific issues
5. **Present status to user** in plain language (not raw code output):
   - "Task 3/7 complete: Created the user authentication endpoint. QA review: passed."
   - If issues found: present them and use `AskUserQuestion`:
     - "Fix the issues and continue"
     - "Skip this issue and continue"
     - "Pause implementation"
6. **Update task status** to `[x]` complete in tasks.md

### Step 4: Post-Implementation

After all tasks are complete:

1. **Update spec status** — mark tasks.md status as "Complete"
2. **Update roadmap** — if `product/roadmap.md` exists, update the feature's status to "Complete"
3. **Update tech stack** — if new technologies were introduced, update `product/tech-stack.md`
4. **Update implementation log** — create or update `implementation-log.md` with:
   - What was built
   - Key decisions made during implementation
   - Any issues encountered and how they were resolved
   - Current product state

5. **Present summary** to the user:
   - What was implemented (in plain language)
   - How many tasks completed
   - Any issues found and resolved
   - Acceptance criteria status (which passed/failed)
   - Suggested next steps

Use `AskUserQuestion`:
- "Run `/specops-implement-next` to implement the next spec"
- "Run `/specops-steer-spec` to review this spec based on implementation learnings"
- "I'm done for now"
