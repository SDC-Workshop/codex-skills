# SDC-Workshop Codex skills

This private repository is the canonical source for the user's maintained Codex skills. The workflow chain is `grill-with-docs → to-spec → to-tickets → implement → code-review`.

The repository contains these five skills:

- `grill-with-docs`
- `to-spec`
- `to-tickets`
- `implement`
- `code-review`

The workflow uses three model-neutral titles. ARCHITECT owns specifications, architecture, contracts, and ticket decomposition. ORCHESTRATOR directs and reviews each ticket. CODING agents alone edit implementation deliverables. The current model assignments, retry limits, fresh ticket contexts, communication rules, and completion gates are defined in [`skills/implement/references/agent-workflow.md`](skills/implement/references/agent-workflow.md).

The upstream interview uses the separately installed `grilling` and `domain-modeling` skills. `tdd` is optional. Install a maintained skill by symlinking its directory from `~/.agents/skills/` to the matching `skills/<name>` directory in this checkout. Keep vendor and system skills separately managed.
