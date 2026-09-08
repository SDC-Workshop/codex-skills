---
name: to-spec
description: Turn agreed decisions into a stable implementation spec and publish it
---

Turn the current conversation and repository understanding into a revisioned implementation spec. Synthesize decisions that have already been made. Do not reopen settled product choices or run a routine interview.

Read and apply [the shared agent workflow](../implement/references/agent-workflow.md) before planning. The full chain is `grill-with-docs -> to-spec -> to-tickets -> implement -> code-review`. This entrypoint keeps the planning boundary visible: Astra, `gpt-6-astra`, is the architect and senior developer who owns specifications, requirements, architecture, contracts, ticket decomposition, and corrections to affected specs and dependent tickets, with the configured Astra effort preserved; Sol, `gpt-5.6-sol`, directs multiple Luna workers, orchestrates, and reviews at `medium`, and owns status and execution records; Luna, `gpt-5.6-luna`, is the only implementation author and uses `medium`, `high`, `xhigh`, or `max` only as the bounded assignment warrants. Astra and Sol may inspect code, run read-only checks, and edit planning or review records, but Luna alone edits implementation code, tests, scripts, runtime configuration, generated code, and integration conflicts. Select identities through supported runtime controls. A role name in prose or an unsupported YAML model field does not configure a model, and an unavailable named role is not silently substituted or granted broader authority.

Here, `accepted` and `approved` mean that the current spec, ADR, or durable workflow record contains product and architecture authority. They do not require a new user approval when no product decision or new external authority remains.

## Process

### 1. Gather context and authority

Use the supplied conversation, referenced documents, and current repository. Explore the repository when needed. Read the domain glossary and relevant ADRs, and use their vocabulary. Inspect existing behavior and tests so the spec can preserve behavior-based checks at the highest useful existing seam.

Treat user decisions and accepted ADRs as authoritative. Do not ask the user to reconfirm test seams, ticket size, or settled decisions. After completing independent work, ask one focused question only for an unresolved product decision or new external authority. Keep unrelated work moving while that answer is pending.

### 2. Establish the planning contract

Give the spec a stable identity and revision, and preserve its lineage when revising it. Assign stable requirement IDs such as `REQ-001`; each requirement describes observable behavior and the evidence that will prove acceptance. Record the contracts and invariants that tickets must preserve, including API, schema, UI, or integration boundaries when they apply.

Group implementation into bounded work packages such as `WP-001`. For each package, state its verifiable outcome, contract ownership, dependencies, scope exclusions, and the acceptance evidence expected from implementation, review, and supervision. Resolve architecture before ticket dispatch. Preserve vertical slices, genuine blockers, behavior-based testing, and the safe expand, migrate, and contract sequence for wide mechanical refactors.

Record the initial implementation budget and the two permitted Astra return cycles for each prospective ticket. A failed Sol verdict pauses further implementation until Astra reviews the evidence. Replacement workers, renaming, splitting, or a fresh context cannot reset a ticket's identity, lineage, attempts, or remaining budget.

### 3. Write the spec

Use a compact, decision-rich structure. Avoid boilerplate-long user-story lists and arbitrary line-count or identical-size quotas.

```markdown
# <Spec title>

Spec revision: <stable revision>
Parent or lineage: <stable identity and predecessor, if any>

## Problem statement
<The user-visible problem>

## Solution
<The intended observable behavior>

## Requirements and acceptance evidence
- REQ-001: <behavior>. Evidence: <specific test, check, or observable result>.

## Work packages
- WP-001: <one bounded outcome>. Requirements: REQ-001. Contracts: <owned and preserved>. Dependencies: <IDs or None>. Excludes: <boundary>.

## Contracts and dependency implications
<Interfaces, invariants, dependency edges, and consequences of changing them>

## Implementation decisions
<Modules, architecture, schemas, APIs, interactions, and relevant ADR decisions>

## Testing decisions
<External behavior, highest existing seams, and prior art in the repository>

## Out of scope
<Explicit exclusions>

## Further notes
<Risks, open product decisions, and handoff notes>
```

Do not include fragile file paths or code snippets unless an existing path or a prototype shape is required to identify a contract. Keep any prototype snippet limited to the decision-rich state machine, reducer, schema, or type shape.

### 4. Publish and hand off

Publish the approved spec to the configured tracker only when that tracker and the user's existing publishing authorization already exist. Apply the configured `ready-for-agent` triage label when the tracker supports it. If tracker setup or authority is absent, preserve a local draft and its stable identity for the caller's configured location. Do not send external comments or publish elsewhere.

Hand `/to-tickets` the complete spec revision, requirement IDs and evidence, work packages, contracts, dependency implications, scope exclusions, open decisions, and acceptance boundary. It must turn each package into a one-outcome ticket that fits one fresh Sol implementation context, including worker handoff and review. Do not close or modify a parent issue as part of spec creation or routine planning. Astra may revise the parent spec and affected dependent tickets during an authorized return cycle.

### 5. Define completion

- Luna completion means the assigned implementation change and evidence are submitted within the ticket boundary. Luna does not close the ticket.
- Sol completion means independent checks of accuracy, documented standards and compliance, completeness, integration, and every required acceptance result. A worker message or green tests alone is insufficient. Sol records the candidate/base evidence, verdict, attempts, and next action.
- Astra completion means the recorded Sol evidence satisfies this spec and its contracts. Astra may correct the spec or affected tickets and must not take over implementation coding or start an open-ended duplicate audit.

If an Astra revision changes a requirement or dependency, put affected downstream tickets on hold, reconcile their contracts and readiness, and invalidate stale affected acceptance evidence while preserving evidence that remains valid.
