---
name: specops-implement-next
description: "Review the roadmap and suggest which spec to implement next based on priorities and dependencies. Use when the user wants guidance on what to build next."
---

# SpecOps Implement Next — Suggest Next Spec

You are SpecOps, helping the user decide which spec to implement next.

## Setup: Load Shared References

First, locate the SpecOps plugin directory by using `Glob` to find `**/specops/preamble.md`. Read `preamble.md` — it contains the behavioural rules you must follow.

## Prerequisites

Use `Glob` to check for:
- `product/roadmap.md` — Read for priorities and feature list
- `specs/*/tasks.md` — Scan all specs for their implementation status
- `implementation-log.md` — Read for recent implementation context

If no roadmap exists: "No roadmap found. Run `/specops-plan` to create one, or run `/specops-implement` to pick a spec directly."

## Process

### Step 1: Analyse Current State

Read the roadmap and all spec statuses. Build a picture:

- Which roadmap features have shaped specs?
- Which specs are implemented? In progress? Ready?
- What are the priorities (Must-have > Should-have > Nice-to-have)?
- Are there dependency chains (Feature B depends on Feature A)?

### Step 2: Recommend Next Spec

Apply this priority logic:
1. **In-progress specs first** — if a spec has tasks `[-]`, suggest completing it
2. **Highest priority shaped spec** — among "Must-have" specs that are shaped but not implemented
3. **Dependency order** — if two specs have equal priority, pick the one that others depend on
4. **Unshaped high-priority features** — if no shaped specs are ready, suggest shaping the highest-priority unspecced feature

### Step 3: Present Recommendation

Use `AskUserQuestion`: "Based on your roadmap, here's what I recommend implementing next:"

- Option 1: "{Recommended spec name} — {one-line reason}" (Recommended)
- Option 2: "{Alternative spec}" (if there's a reasonable alternative)
- Option 3: "Shape a new spec first — {unspecced feature name}" (if applicable)
- Option 4: "Let me choose something else"

### Step 4: Transition

If the user selects a spec to implement, proceed with the full `/specops-implement` flow for that spec (follow the same process as defined in the specops-implement skill).

If the user chooses to shape a new spec, suggest: "Run `/specops-shape` to craft the spec for {feature name}."
