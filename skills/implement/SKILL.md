---
name: implement
description: Implement one ticket from its recorded spec
---

# Implement a ticket

ARCHITECT, ORCHESTRATOR, and CODING are model-neutral authority titles. ARCHITECT owns the spec, architecture, ticket, contracts, and revisions. ORCHESTRATOR directs the active ticket and reviews its candidate. CODING agents alone edit implementation deliverables. Apply the current runtime profile from the [shared agent workflow](references/agent-workflow.md). ARCHITECT and ORCHESTRATOR do not implement fixes.

Work one ticket whose product and architecture are recorded in its current spec
and ticket revision, using one fresh ORCHESTRATOR context. The ticket must have one
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
missing, record the blocker and send it to ORCHESTRATOR for ARCHITECT. Do not reopen settled
test seams or ticket sizing.

Before dispatch, recover the durable record. It must preserve stable ticket
identity and lineage, spec and ticket revisions, owner and active workers,
dependency state, retry_policy, execution_round, round_attempts_started, attempts_started, architect_returns_used, remaining budget,
base and candidate evidence, changed files, required checks, check results,
acceptance evidence, separate Standards and Spec verdicts, and next action.
Missing state comes from durable history or is reported unavailable. Never
guess a missing counter as zero.

## Dispatch and implement

ORCHESTRATOR gives each CODING worker exactly the six labels Goal, Current state,
Relevant files, Constraints, Done when, Checks to run. Include actual model
and effort, ticket revision and attempt, accepted evidence, remaining budget,
exclusive file ownership, and the supervisor identity plus the runtime-bound
ORCHESTRATOR handle supplied by the current invocation, interface contracts, and
authority inside those labels. ORCHESTRATOR dispatches a new or existing CODING worker
through the supported spawn or same-ticket follow-up control.
The full six-label contract is in the shared workflow.

ORCHESTRATOR may direct several CODING workers inside this one ticket when their
assignments are independent. ORCHESTRATOR bounds concurrency to available runtime
slots, gives each worker exclusive files or a separate worktree, and
serializes shared-file edits and integration. CODING acknowledges ownership
before editing and sends meaningful progress, interface proposals, blockers,
and completion evidence through collaboration.send_message to the bound ORCHESTRATOR
handle recorded in that handoff. A peer or app-thread delivery does not
replace that route. A peer cannot grant scope, ownership, or contract changes.

CODING is the only author of implementation code, tests, scripts, runtime
configuration, generated code, and integration conflict resolutions. Keep
changes inside the current ticket and owned files. ARCHITECT and ORCHESTRATOR may inspect
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
interface, stop the shared edit and send a concrete proposal to ORCHESTRATOR, with
peers copied or informed promptly. ORCHESTRATOR reconciles the proposal with the
recorded contract, asks ARCHITECT to approve any ticket, dependency, or contract
change, and directs a CODING integrator for the serialized edit. Do not let
peer agreement silently change the ticket.

## Submit the candidate and review it

Before submission, finish all CODING writes and freeze every candidate writer.
Record the resolved starting base, a deterministic candidate manifest and
content digest, every tracked changed file, every new file, and the comparison
semantics. The manifest excludes pre-existing user changes. Invoke
[code-review](../code-review/SKILL.md) on the exact frozen candidate. When
review happens before commit, the candidate includes uncommitted tracked
changes and new files. A branch name alone is not immutable review evidence.
ORCHESTRATOR rechecks the frozen writer state and the same candidate digest before its
review verdict. ARCHITECT rechecks that same digest before acceptance. Any drift
invalidates the candidate and follows the failure path.

ORCHESTRATOR owns separate Standards and Spec verdicts and independently checks
accuracy, documented standards and compliance, completeness, integration,
all acceptance criteria, and every required check. A CODING completion message
or green tests alone does not pass the ticket. The candidate remains within
the current attempt until ORCHESTRATOR records the verdict and evidence. Code-review
does not edit or commit the candidate.

CODING is done when the owned candidate and its evidence are submitted. ORCHESTRATOR is
done when the candidate, checks, contracts, integration, and acceptance
evidence are independently verified and recorded and the record is sent to
ARCHITECT. ARCHITECT is done when it accepts that exact frozen candidate against the
current spec and contracts. Keep the original local-commit behavior unless
the user or repository instructions override it. Only then does ORCHESTRATOR assign
the required local commit to a CODING integrator.

The CODING integrator stages only candidate-owned paths, including owned new
files, and excludes pre-existing user changes. The integrator verifies the
commit identity, changed-file list, and commit-tree content digest against the
frozen candidate and records them. If the commit cannot be isolated safely,
ORCHESTRATOR records a blocker. ORCHESTRATOR records completion, persists the next-ticket
handoff, and ends the workers only after the verified commit and ARCHITECT
acceptance. Keep the commit or working-tree candidate identity in the durable
record.

## Failed attempts and revisions

Each execution round permits two CODING attempts. After the first failed attempt, ORCHESTRATOR records the defects and directs a focused second attempt under the current ticket. After the second failed attempt in that round, ORCHESTRATOR applies the shared conditional return rule: use a remaining ARCHITECT return or mark the ticket exhausted.

ARCHITECT approves or revises the ticket, affected contracts, and dependent tickets, then returns it to ORCHESTRATOR. A ticket may return to ARCHITECT at most twice. The second return opens the final round, so the full ceiling is six attempts. If attempt 6 fails, mark the ticket blocked and exhausted without another return. Persist `execution_round`, `round_attempts_started`, `attempts_started`, and `architect_returns_used`; replacement workers, splitting, renaming, or context changes never reset them. Migrate a legacy `astra_returns_used` value without resetting it.

Proceed without routine user approval when the current spec already authorizes the work. Ask only when independent work leaves an unresolved product decision or requires new external authority.
