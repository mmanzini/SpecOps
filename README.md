# SpecOps

A Claude Code plugin for PM-first Spec-Driven Development. SpecOps guides you through the full product lifecycle — from defining your product vision to implementing features with AI agent teams.

Built on the [SDD Standard](https://github.com/your-org/sdd-standard) v0.1 methodology.

## Installation

### Local (for development or testing)

```bash
claude --plugin-dir /path/to/SpecOps
```

### From GitHub

```bash
# Add the marketplace (one-time)
/plugin marketplace add your-org/specops

# Install the plugin
/plugin install specops@specops
```

After installation, run `/specops` in any project to get started.

## Commands

| Command | Description |
|---|---|
| `/specops` | Project dashboard — shows current state and suggests next action |
| `/specops-brief` | Define your product brief (problem, users, solution, metrics) |
| `/specops-steer-brief` | Review and refine an existing product brief |
| `/specops-plan` | Build your roadmap and tech stack |
| `/specops-steer-plan` | Review and refine roadmap and tech stack |
| `/specops-shape` | Shape a feature spec (requirements, design, tasks) |
| `/specops-steer-spec` | Review and refine an existing spec |
| `/specops-implement` | Execute a spec with an AI agent team |
| `/specops-implement-next` | Suggest which spec to implement next |

## How It Works

### The 4-Phase Model

```
Brief  →  Plan  →  Shape  →  Implement
```

| Phase | Scope | What You Get |
|---|---|---|
| **Brief** | Product-wide | Product brief with mission, problem, users, success metrics |
| **Plan** | Product-wide | Prioritised roadmap, tech stack, project conventions |
| **Shape** | Per-feature | Complete spec: requirements, design, and task breakdown |
| **Implement** | Per-feature | AI agent team executes your spec task by task |

### Expert Agent Consultation

At every major decision point, you can consult a domain expert — an AI agent with a specific professional persona:

- **PM Sparring Partner** — challenges your thinking, pressure-tests decisions
- **Market Analyst** — researches competitors and validates the problem space
- **Product Strategist** — helps prioritise features and define MVP boundaries
- **UX Researcher** — validates user stories and explores user needs
- **Tech Architect** — evaluates stack options and reviews feasibility
- **QA Engineer** — stress-tests acceptance criteria and finds edge cases
- **Devil's Advocate** — challenges assumptions and flags risks
- **Security Engineer** — reviews security implications and compliance

### What SpecOps Generates

When you use SpecOps in a project, it creates and manages these files:

```
your-project/
  CLAUDE.md                     # Steering document (conventions, boundaries)
  product/
    product-brief.md            # Product mission, problem, users
    roadmap.md                  # Prioritised feature list with status
    tech-stack.md               # Technology choices with rationale
  implementation-log.md         # Running context for AI session continuity
  specs/
    feature-name/
      brief.md                  # Feature-level problem and scope
      requirements.md           # User stories, acceptance criteria
      design.md                 # Architecture and design decisions
      tasks.md                  # Ordered implementation tasks
  standards/                    # (optional) Project-wide coding conventions
```

## Quick Start

1. Install the plugin (see Installation above)
2. Open Claude Code in your project directory
3. Run `/specops` to see the dashboard
4. Run `/specops-brief` to define your product
5. Run `/specops-plan` to build your roadmap
6. Run `/specops-shape` to spec your first feature
7. Run `/specops-implement` to build it with an agent team

## Key Principles

- **Never a blank slate** — SpecOps always suggests answers you can confirm, adjust, or override
- **Human decides, AI executes** — every action requires your approval
- **Smarter specs, not longer specs** — concise, actionable output
- **Expert on demand** — consult domain experts at any decision point
- **Session resilient** — progress is saved so you can resume interrupted flows

## Requirements

- Claude Code CLI or Claude Cowork
- No additional dependencies

## License

MIT
