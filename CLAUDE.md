# SpecOps

## Project Overview

SpecOps is a Claude Code skills suite that guides users through the full product lifecycle using Spec-Driven Development (SDD). It provides `/specops-*` slash commands for 4 phases: Brief, Plan, Shape, and Implement. The target user is a PM with enough technical comfort to use CLI or Claude Cowork.

## Tech Stack

- **Format:** Markdown (`.md`) for all generated artifacts and skill definitions
- **Platform:** Claude Code CLI / Cowork
- **Plugin system:** Claude Code plugin with `skills/[skill-name]/SKILL.md` and `.claude-plugin/plugin.json`
- **Methodology:** Built on SDD Standard v0.1

## Project Structure

```
SpecOps/
  .claude-plugin/
    plugin.json                 # Plugin manifest (name, version, description)
  skills/                       # Claude Code skills (one per command)
    specops/SKILL.md            # /specops — home screen
    specops-brief/SKILL.md      # /specops-brief
    specops-steer-brief/SKILL.md
    specops-plan/SKILL.md
    specops-steer-plan/SKILL.md
    specops-shape/SKILL.md
    specops-steer-spec/SKILL.md
    specops-implement/SKILL.md
    specops-implement-next/SKILL.md
  specops/                      # Shared references (read by skills via Glob discovery)
    preamble.md                 # Common behavioural rules
    experts.md                  # Expert persona definitions
    templates/                  # Output file templates
  CLAUDE.md                     # This file
  Project Spec.md               # Product specification
```

## Installation

**Local development:**
```bash
claude --plugin-dir ./path/to/SpecOps
```

**From GitHub (once published):**
```bash
/plugin marketplace add your-org/specops
/plugin install specops@specops
```

## File Generation Conventions

When SpecOps commands generate files for the user's project, they follow this structure:

```
user-project/
  CLAUDE.md                     # Steering document (project-wide)
  product/
    product-brief.md            # Product mission, vision, problem, users
    roadmap.md                  # Prioritised feature list with status
    tech-stack.md               # Technology choices with rationale
  implementation-log.md         # Running context for AI session continuity
  specs/
    feature-name/               # One folder per spec (no date prefix)
      brief.md                  # Feature-level problem and scope
      requirements.md           # User stories, acceptance criteria, boundaries
      design.md                 # Architecture and design decisions
      tasks.md                  # Ordered atomic tasks with status
    bugfix-name/
      bugfix.md                 # Current vs expected vs unchanged behaviour
      tasks.md                  # Fix implementation tasks
  standards/                    # (optional) Project-wide coding conventions
```

## Boundaries

### Always

- Read `specops/preamble.md` at the start of every skill execution
- Use `AskUserQuestion` for all user interactions (never open-ended chat questions)
- Offer expert consultation contextually (1-2 relevant experts, not all 8)
- Use targeted plan mode (only at file-generation checkpoints)
- Update source of truth files after generating or modifying outputs
- Scan existing project state before proposing changes

### Ask

- Before changing the file generation structure or template format
- Before adding new expert personas
- Before modifying the preamble or shared behavioural rules

### Never

- Auto-execute implementation without explicit user approval
- Present a blank slate — always offer suggested answers
- Enter plan mode for the entire flow (only at file-generation checkpoints)
- Spawn expert agents without including full context in the Agent prompt
