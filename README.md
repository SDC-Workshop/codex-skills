# SDC-Workshop Codex skills

This private repository is the canonical source for the user's maintained Codex skills. The workflow chain is `grill-with-docs -> to-spec -> to-tickets -> implement -> code-review`.

The repository contains these five skills:

- `grill-with-docs`
- `to-spec`
- `to-tickets`
- `implement`
- `code-review`

Astra uses gpt-6-astra and owns specifications, architecture, contracts, ticket decomposition, and corrections. Sol uses gpt-5.6-sol at medium effort and directs and reviews each ticket. Luna uses gpt-5.6-luna and is the implementation author. Retry limits, fresh ticket contexts, evidence, and the no-supervisor-code-edit boundary are defined in [`skills/implement/references/agent-workflow.md`](skills/implement/references/agent-workflow.md).

The upstream interview uses the separately installed `grilling` and `domain-modeling` skills. `tdd` is optional. Install a maintained skill by symlinking its directory from `~/.agents/skills/` to the matching `skills/<name>` directory in this checkout. Keep vendor and system skills separately managed.
