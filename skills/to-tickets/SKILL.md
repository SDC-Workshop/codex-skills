---
name: to-tickets
description: Break a spec into one-outcome tickets with durable dependencies
---

# To tickets

Break an accepted plan or spec into implementation tickets. `to-tickets` is the canonical installed name. If the user says `/to-ticket`, interpret that singular term as their request for this canonical workflow; do not promise a separate registered command. Read and apply [the shared agent workflow](../implement/references/agent-workflow.md) before ticket planning. The full chain is `grill-with-docs -> to-spec -> to-tickets -> implement -> code-review`.

Here, `accepted` and `approved` mean that the current spec, ADR, or durable workflow record contains product and architecture authority. They do not require a new user approval when no product decision or new external authority remains.

This entrypoint keeps the boundary visible: Astra, `gpt-6-astra`, is the architect and senior developer who owns specifications, architecture, contracts, ticket decomposition, and corrections to affected specs and dependent tickets, with the configured Astra effort preserved; Sol, `gpt-5.6-sol`, directs multiple Luna workers, orchestrates, reviews, and submits evidence to Astra at `medium`, and owns ticket status and execution records; Luna, `gpt-5.6-luna`, is the only implementation author and uses `medium`, `high`, `xhigh`, or `max` only as the bounded assignment warrants. Astra and Sol may inspect code, run read-only checks, and edit planning or review records, but Luna alone edits implementation code, tests, scripts, runtime configuration, generated code, and integration conflicts. Select identities through supported runtime controls. A role name in a prompt or an unsupported YAML field does not configure a model, and an unavailable named role is not silently substituted or granted broader authority.

## Process

### 1. Gather context

Work from the accepted spec revision and the conversation context. If the user supplies a spec path, issue number, or URL, read its full body and comments through the configured source. Explore the repository when needed. Use the domain glossary and relevant ADRs, inspect existing behavior and tests, and preserve the highest useful test seam.

Do not repeat a user quiz about ticket granularity or blocking edges. The accepted spec is the product and architecture decision. After independent work, ask only about an unresolved product decision or new external authority. If the spec is missing required contracts or contains a conflict, report the concrete gap to Astra and hold the affected package rather than inventing a decision.

### 2. Validate the package frontier

Every package must have stable requirement IDs, acceptance evidence, a spec revision, resolved contracts, explicit exclusions, and dependency implications before dispatch. Keep one verifiable outcome per ticket and size it for one fresh Sol implementation context, including worker handoff, `/code-review`, supervisory acceptance, and completion recording. Aim for comparable, manageable complexity. Sol may narrow a worker assignment inside the ticket; Astra approves ticket or dependency changes.

Preserve vertical slices that are independently demoable or verifiable. Keep prefactoring inside a real end-to-end slice when possible. A wide mechanical refactor is sequenced as expand, migrate batches, and contract. Each migrate batch remains green while the old form exists, and the contract ticket blocks on every migrate batch. If batches cannot stay green independently, use an integration-and-verify ticket that all relevant batches block.

### 3. Draft durable tickets

Use one stable ticket identity per outcome and preserve its lineage across revisions, workers, renaming, splitting, and fresh contexts. Record the spec revision, requirement IDs, contract and dependency state, ownership, candidate/base revision or patch evidence, check results, status, attempts started, Astra returns used, remaining budget, and next action before dispatch or review transitions. Missing state must be recovered from durable history or reported unavailable, never guessed as zero.

Each worker handoff has exactly these six labels: `Goal`, `Current state`, `Relevant files`, `Constraints`, `Done when`, and `Checks to run`. Include the actual Luna model and effort, ticket revision and attempt, accepted evidence, remaining budget, exclusive file ownership, Sol supervisor, Astra authority, and interface contracts in those fields. Independent Luna workers may run concurrently only within one ticket and with exclusive files or separate worktrees; serialize shared-file edits. Sol reconciles interfaces and directs an integrator when needed.

Peer workers may communicate about interfaces with Sol copied or promptly informed. Peers cannot grant scope or ownership changes. Sol verifies the actual combined changes and checks before sending evidence to Astra.

```markdown
# <NN> - <Ticket title>

Spec revision: <stable revision>
Lineage: <stable ticket identity and predecessor, if any>
Requirement IDs: <REQ IDs>

What to build: <one observable end-to-end outcome>

Blocked by: <ticket numbers and stable identities, or "None, can start immediately">

Contracts and dependency state: <interfaces and exact readiness implications>
Scope exclusions: <what this ticket does not change>
Ownership and review: Luna <model/effort>; Sol <supervisor>; Astra <authority>
Attempt state: initial pass or return cycle; started <count>; Astra returns used <count>/2; remaining budget <count>
Status: ready-for-agent, in progress, on hold, blocked, or exhausted

## Handoff to implement

Goal: <the one outcome>
Current state: <accepted evidence, base revision, and dependency readiness>
Relevant files: <owned paths or stable modules; keep ownership exclusive>
Constraints: <contracts, exclusions, authority, model/effort, revision, attempt, and budget>
Done when: <Luna evidence, Sol checks and verdict, and Astra acceptance record>
Checks to run: <exact commands, or the permitted way to resolve current repository commands at dispatch>

## Acceptance criteria

- [ ] <objective behavior and evidence for each linked requirement>
```

The six handoff labels are the worker interface. Keep acceptance criteria objective and traceable to requirements. Avoid stale implementation details and code snippets except for a decision-rich prototype shape needed to define a contract.

### 4. Publish tickets and work the frontier

Publish only with existing tracker setup and the user's publishing authority. If no tracker is configured or authority is absent, write a local draft with one file per ticket under `.scratch/<feature-slug>/issues/`, or the configured tracker-equivalent location. Number local files from `01` in dependency order. Preserve native blocking links on a real tracker, or list stable ticket identities in each local `Blocked by` field. Apply `ready-for-agent` when the configured tracker supports it. Do not send external comments or publish without that authority. Do not close or modify a parent issue as part of ticket creation or routine ticket execution. Astra may revise the parent spec and affected dependent tickets during an authorized return cycle.

Work the frontier one ticket per fresh context, clearing between them.

Only a ticket whose blockers are complete is eligible. Before dispatch, persist its identity, revision, attempt transition, owner, dependencies, base/candidate state, checks, and next action. After each individual ticket passes implementation, `/code-review`, supervisory acceptance, and completion recording, persist a handoff and end that ticket's workers before starting `/implement` on the next eligible ticket. Carry only the next ticket, spec/ADR references, compact dependency and budget state, repository revision, and relevant accepted evidence into the next context.

Use a supported fresh invocation such as `fork_turns: "none"` with an explicit supported model and effort, or a new isolated Sol invocation with no inherited transcript. A follow-up to Sol, a model change, summarization, or compaction is not a fresh ticket context. Do not clear mid-ticket. If the runtime cannot start a fresh invocation, save the handoff and report that exact capability gap without claiming a reset.

Do not run two tickets concurrently. Do not clear context while workers are writing the current ticket.

An unavoidable restart carries the same ticket identity, revision, attempt counters, current candidate or base evidence, ownership, and next action. It does not count as a new ticket or reset its budget. Do not create an unsolicited user-facing task to work around the fresh-context boundary.

### 5. Handle failure, returns, and dependency changes

An attempt is one planned Luna coding pass followed by one submitted candidate and Sol's required review. Normal build, test, and debug iterations before submission are part of that pass. A failed Sol verdict or incomplete attempt pauses implementation until Astra reviews Sol's failure evidence and proposed change. A return cycle includes that evidence, Astra's approval or modification of the ticket and affected dependent tickets, and re-dispatch to Sol. Permit one initial pass and at most two Astra return cycles per ticket, for three total attempts. Increment counters before dispatch and review transitions. A supervisor rejection follows the same failure path. A replacement worker or split cannot reset the budget.

After the third failed attempt, mark the ticket blocked and exhausted, preserve the smallest unresolved issue and all evidence, block dependents, and continue only independently eligible frontier tickets. A later retry requires new bounded authorization. During an Astra revision, hold affected downstream tickets, reconcile contracts and readiness, and invalidate stale affected acceptance evidence while preserving evidence that remains valid.

Define done separately. Luna submits owned changes and evidence. Sol independently verifies accuracy, standards and compliance, completeness, integration, and required acceptance evidence. Astra checks Sol's completed record against the spec and accepts or returns it without coding. A worker completion message or green tests alone never closes a ticket.
