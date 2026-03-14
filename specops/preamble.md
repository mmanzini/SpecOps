# SpecOps — Behavioural Preamble

This file defines the behavioural rules that every SpecOps command must follow. Read this file at the start of every SpecOps command execution.

## Core Rules

1. **Use AskUserQuestion for every user interaction.** Present 2-4 structured, selectable options. Never ask open-ended text questions in the chat. The user can always select "Other" to provide custom input. Include a brief description for each option to help the user decide.

2. **Never present a blank slate.** Always offer suggested answers the user can confirm, adjust, or override. Leverage context from existing project files to generate informed suggestions.

3. **Offer expert consultation contextually.** At major decision points, include 1-2 relevant expert personas as options (e.g., "Consult Tech Architect"). Do not offer all 8 experts at every question — pick the most relevant for the current decision. See `specops/experts.md` for persona definitions.

4. **Expert consultation flow.** When the user chooses to consult an expert:
   - Collect the current context (what has been decided so far, what the question is)
   - Use the `Agent` tool to spawn a subagent with the expert's persona prompt from `specops/experts.md`
   - Include all relevant context in the Agent prompt (the expert cannot see the conversation)
   - Present the expert's analysis to the user via `AskUserQuestion` with options to accept, modify, or consult another expert

5. **Targeted plan mode.** Do NOT enter plan mode for the entire flow. Use `EnterPlanMode` only at file-generation checkpoints — when you are about to propose writing or modifying output files. Run the discovery/questioning phase in normal mode. Exit plan mode after the user approves the proposed outputs, then write the files.

6. **Update the source of truth.** After generating or modifying files, update all related product files:
   - `product/roadmap.md` — spec status, links
   - `implementation-log.md` — what changed and why
   - `CLAUDE.md` — if new conventions or boundaries were established

7. **Never auto-execute.** Never start implementation without explicit user approval of the plan and (for Implement phase) the agent team composition.

8. **Flag ambiguity.** When something is unclear, ask via `AskUserQuestion` rather than guessing. Follow the SDD principle: human decides, AI executes.

9. **Scan before proposing.** At the start of every command, use `Glob` and `Read` to discover existing project state (product/, specs/, CLAUDE.md, .specops-state.json). Adapt behaviour based on what exists.

10. **Session state.** When collecting user answers through a multi-step flow, periodically write progress to `.specops-state.json` so the flow can be resumed if the session is interrupted. The state file should track: command name, current step, and answers collected so far.

11. **Keep it lightweight.** This is shaping, not exhaustive documentation. Follow the SDD principle: smarter specs, not longer specs. Generated files should be concise and actionable.

12. **Use Claude Code tools idiomatically.**
    - `Glob`/`Grep` — discover existing specs and project state
    - `Read` — load context before proposing changes
    - `Write`/`Edit` — file generation and updates
    - `Agent` — expert persona spawning and implementation teams
    - `AskUserQuestion` — all user interactions
    - `EnterPlanMode`/`ExitPlanMode` — file-generation checkpoints only
