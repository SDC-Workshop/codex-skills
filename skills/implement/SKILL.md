---
name: implement
description: Implement one ticket from its recorded spec
---

# Implement a ticket

The implementation owner is Luna, model gpt-5.6-luna. Sol, model
gpt-5.6-sol at medium effort, orchestrates the active ticket, owns its status
and execution record, and reviews the candidate. Astra, model gpt-6-astra,
owns the specification, architecture, ticket, contracts, and any revision.
Select these identities through supported runtime controls. A role name in a
prompt or a model field in skill metadata does not select a model.

Work one ticket whose product and architecture are recorded in its current spec
and ticket revision, using one fresh Sol context. The ticket must have one
verifiable outcome, expected behavior, scope exclusions, contracts,
dependencies, planned worker boundaries, acceptance criteria, and exact checks
or a permitted way to resolve current repository commands. Resolve architecture
and dependency implications before dispatch. Do not start a second ticket or
clear the context while workers are writing the current ticket.

Read and apply the detailed retry, durable-record, handoff, communication,
concurrency, restart, candidate-integrity, commit, and supervisory-acceptance
rules in the [shared agent workflow](references/agent-workflow.md) for every
ticket. Here, recorded product or architectural authority in the current spec
is what accepted or approved means. Do not require fresh user approval when no
product decision or new external authority remains.

## Gather the ticket and recover state

Read the supplied ticket, its spec revision, linked requirements and
acceptance evidence, relevant ADRs, and the current repository state. Confirm
that dependencies are ready and that the worker boundaries and ownership are
unambiguous. Use the repository's glossary and existing behavior-based test
seams. If a required product decision, contract, dependency, or authority is
missing, record the blocker and send it to Sol for Astra. Do not reopen settled
test seams or ticket sizing.

Before dispatch, recover the durable record. It must preserve stable ticket
identity and lineage, spec and ticket revisions, owner and active workers,
dependency state, attempts_started, astra_returns_used, remaining budget,
base and candidate evidence, changed files, required checks, check results,
acceptance evidence, separate Standards and Spec verdicts, and next action.
Missing state comes from durable history or is reported unavailable. Never
guess a missing counter as zero.

## Dispatch and implement

Sol gives each Luna worker exactly the six labels Goal, Current state,
Relevant files, Constraints, Done when, Checks to run. Include actual model
and effort, ticket revision and attempt, accepted evidence, remaining budget,
exclusive file ownership, and the supervisor identity plus the runtime-bound
Sol handle supplied by the current invocation, interface contracts, and
authority inside those labels. Sol dispatches a new or existing Luna worker
through the supported spawn or same-ticket follow-up control.
The full six-label contract is in the shared workflow.

Sol may direct several Luna workers inside this one ticket when their
assignments are independent. Sol bounds concurrency to available runtime
slots, gives each worker exclusive files or a separate worktree, and
serializes shared-file edits and integration. Luna acknowledges ownership
before editing and sends meaningful progress, interface proposals, blockers,
and completion evidence through collaboration.send_message to the bound Sol
handle recorded in that handoff. A peer or app-thread delivery does not
replace that route. A peer cannot grant scope, ownership, or contract changes.

Luna is the only author of implementation code, tests, scripts, runtime
configuration, generated code, and integration conflict resolutions. Keep
changes inside the current ticket and owned files. Astra and Sol may inspect
those files and run read-only checks. They may edit planning, review, and
documentation-only planning records, but they do not apply implementation
changes or fixes.

Run the ticket's exact checks. At an agreed existing seam, use /tdd when the
ticket calls for test-first work. The ticket's behavior-based checks and
highest existing seam remain authoritative. Use typechecking and focused
behavior checks during normal iterations, then run the required full suite or
other final checks before submission. Normal build, test, and debugging
iterations belong to the current attempt. Keep the evidence tied to the
behavior and the highest useful existing seam.

If independent workers discover that they need the same shared schema or
interface, stop the shared edit and send a concrete proposal to Sol, with
peers copied or informed promptly. Sol reconciles the proposal with the
recorded contract, asks Astra to approve any ticket, dependency, or contract
change, and directs a Luna integrator for the serialized edit. Do not let
peer agreement silently change the ticket.

## Submit the candidate and review it

Before submission, finish all Luna writes and freeze every candidate writer.
Record the resolved starting base, a deterministic candidate manifest and
content digest, every tracked changed file, every new file, and the comparison
semantics. The manifest excludes pre-existing user changes. Invoke
[code-review](../code-review/SKILL.md) on the exact frozen candidate. When
review happens before commit, the candidate includes uncommitted tracked
changes and new files. A branch name alone is not immutable review evidence.
Sol rechecks the frozen writer state and the same candidate digest before its
review verdict. Astra rechecks that same digest before acceptance. Any drift
invalidates the candidate and follows the failure path.

Sol owns separate Standards and Spec verdicts and independently checks
accuracy, documented standards and compliance, completeness, integration,
all acceptance criteria, and every required check. A Luna completion message
or green tests alone does not pass the ticket. The candidate remains within
the current attempt until Sol records the verdict and evidence. Code-review
does not edit or commit the candidate.

Luna is done when the owned candidate and its evidence are submitted. Sol is
done when the candidate, checks, contracts, integration, and acceptance
evidence are independently verified and recorded and the record is sent to
Astra. Astra is done when it accepts that exact frozen candidate against the
current spec and contracts. Keep the original local-commit behavior unless
the user or repository instructions override it. Only then does Sol assign
the required local commit to a Luna integrator.

The Luna integrator stages only candidate-owned paths, including owned new
files, and excludes pre-existing user changes. The integrator verifies the
commit identity, changed-file list, and commit-tree content digest against the
frozen candidate and records them. If the commit cannot be isolated safely,
Sol records a blocker. Sol records completion, persists the next-ticket
handoff, and ends the workers only after the verified commit and Astra
acceptance. Keep the commit or working-tree candidate identity in the durable
record.

## Failed attempts and revisions

An attempt is one planned Luna pass, one submitted candidate, and Sol's
required review. If Sol returns a failure or the attempt cannot complete, Sol
checks the remaining return budget before sending anything to Astra. If
attempts_started is already 3 or astra_returns_used is already 2, Sol marks
the ticket blocked and exhausted directly, preserves the smallest unresolved
issue, blocks dependents, and creates no third return cycle. Otherwise Sol
increments astra_returns_used on entry to the Astra return cycle, records the
failure and smallest proposed change, pauses implementation, and sends one
concrete recommendation to Astra. A Luna worker must not patch immediately.

The initial dispatch starts attempt one. A ticket permits at most two Astra
return cycles and three total attempts. Astra's return must approve or modify
the ticket, affected dependent tickets, and any spec or contract revision
before Sol writes a new handoff and increments attempts_started immediately
before re-dispatching Luna. Replacement workers, a fresh context, renaming, or
splitting cannot reset the identity, lineage, attempt count, or return budget.

If Astra changes a requirement, output contract, dependency, or acceptance
boundary, Sol puts affected downstream tickets on hold, reconciles their
contracts and readiness, and invalidates stale acceptance evidence before
release. Preserve accepted evidence that remains valid. Record the revised
spec or ticket identity, surviving evidence, stale evidence, and next action.

Do not ask for routine user approval on a successful ticket. Ask only when
independent work leaves an unresolved product decision or requires new
external authority.
