---
name: grill-with-docs
description: Interview a plan while recording its glossary and ADR decisions
---

Run a `/grilling` session with `/domain-modeling` to sharpen product direction. Keep the interview: this is the user-facing product-decision stage, before specification and ticket decomposition. Record glossary and ADR decisions that have product and architecture authority, surface unresolved product choices, and hand settled decisions to `/to-spec` for synthesis. Do not add a new user approval gate when no product decision or new external authority remains.

Read and apply [the shared agent workflow](../implement/references/agent-workflow.md) before handing off. The full chain is `grill-with-docs → to-spec → to-tickets → implement → code-review`. The chain uses the model-neutral ARCHITECT, ORCHESTRATOR, and CODING roles defined in the shared workflow. Apply its current runtime profile. Hand settled decisions from `/to-spec` into `/to-tickets`, which gives each ticket one fresh ORCHESTRATOR context and clears between tickets. Runtime controls select these identities. Do not treat a role name in prose as model configuration.
