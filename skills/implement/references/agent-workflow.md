# Shared agent workflow

This is the detailed workflow contract for the spec, ticket, implementation, and
review skills. The entrypoints keep their role, ownership, authority, and
fresh-ticket boundary visible and link here for the shared rules. Apply this
contract to local ticket files or to the configured tracker equivalent.

The chain is grill-with-docs -> to-spec -> to-tickets -> implement ->
code-review. Every entrypoint in that chain must read and apply this shared
workflow. In this document, accepted and approved mean that the current spec
or ticket records the product or architectural decision as authoritative. They
do not require a fresh user confirmation when no product decision or new
external authority remains.

## Roles, authority, and runtime selection

The titles ARCHITECT, ORCHESTRATOR, and CODING describe authority, not model identity. ARCHITECT owns requirements, specifications, architecture, contracts, ticket decomposition, and revisions to affected specs and dependent tickets. ORCHESTRATOR directs the active ticket, coordinates CODING agents, owns status and review, and checks accuracy, compliance, and completeness. CODING agents alone edit implementation code, tests, scripts, runtime configuration, generated code, conflicts, and implementation deliverables. ARCHITECT and ORCHESTRATOR may inspect code and edit planning or review records, but they do not implement fixes.

The current replaceable runtime profile assigns ARCHITECT to `gpt-6-astra` at medium effort. It assigns ORCHESTRATOR to `gpt-6-astra` at low effort or `gpt-5.6-sol` at medium effort, with Sol medium as the default. It assigns CODING agents to `gpt-5.6-luna` at medium, high, xhigh, or max effort, with medium as the default. These are current assignments, not role definitions. Another model or provider may fill any role when the configured profile grants that role and the model can perform it. Select and verify profiles through supported runtime controls. Record the actual provider, model, effort, invocation identity, and dynamic handles in the ticket record. ARCHITECT and ORCHESTRATOR must be separate invocations even when both use the same model family.

Choose model capability and reasoning effort separately for each assignment within supported profile settings. Use the least expensive suitable assignment; a role title does not imply high reasoning effort. Diagnose unclear requirements, environment failures, and ownership conflicts before escalating model cost. Record routing as requested, confirmed by runtime evidence, or unavailable. A model self-report is not runtime confirmation.

A future model or provider change updates this profile without changing the role definitions. Do not put model selectors in skill metadata or silently grant a substitute model broader authority.

## When this workflow applies

Before starting coordination, identify the requested deliverable and choose the smallest sufficient process. Invoking one skill activates that stage and its required checks, not every stage of the chain. Use the full chain when the user requests it or the implementation requires its planning and coordination. Simple tasks do not require multiple agents merely because those agents are available. Do not activate or simulate the chain merely because a task mentions agents, roles, models, tickets, retries, or these skill files. Editing the workflow documentation is a direct documentation task unless the user explicitly requests a multi-agent run. Match the execution method to the requested deliverable and avoid auxiliary specs, tickets, agents, and review ceremonies that do not improve that deliverable.

## Ticket and context boundary

Run one active ticket at a time. A ticket must have one verifiable outcome, a
current spec and ticket revision with recorded product and architectural
authority, explicit expected behavior and exclusions, contracts, dependencies,
worker boundaries, acceptance criteria, and exact checks or a permitted way to
resolve current repository commands at dispatch.
Resolve architecture and dependency implications before dispatch. The ORCHESTRATOR context for that ticket is fresh, and all worker handoffs fit inside it.

When a ticket is ready, recover its durable record before dispatch. Work starts
only when its dependencies are satisfied and the record says the ticket is
eligible. ORCHESTRATOR can narrow an already recorded worker assignment inside the
ticket. ARCHITECT approves a ticket, contract, or dependency change.

After a ticket passes implementation, code review, ORCHESTRATOR's supervisory
acceptance, ARCHITECT's acceptance, required commit verification, and completion
recording, persist a handoff and end that ticket's workers before starting
implementation on the next eligible ticket. ARCHITECT's acceptance must precede
successful completion recording, worker shutdown, and the next-ticket
transition. Do the same durable transition when a ticket becomes blocked or
exhausted, but do not call that transition successful completion. Carry only
the next ticket, the spec and ADR references, compact dependency and budget
state, repository revision, and relevant accepted evidence into the next
context. Do not run two tickets concurrently or clear the context while a
worker is still writing the current ticket.

Use a supported context reset or a new isolated ORCHESTRATOR invocation with no inherited transcript for the next ticket. The collaboration runtime supports
fork_turns: "none" with an explicit model and effort. Reusing ORCHESTRATOR with a
follow-up, changing models, summarizing, or relying on compaction does not make
a fresh ticket context. Do not create an unsolicited user-facing task to work
around this boundary. If the runtime cannot start a real fresh invocation,
save the handoff and report that precise capability gap. A written file does
not prove that a reset occurred.

Do not deliberately clear context mid-ticket. An unavoidable mid-ticket
restart keeps the same ticket, attempt, candidate, ownership, counters, and
next action. End or coordinate active workers before
dispatching a replacement. If a worker is still running, preserve its identity
and exclusive ownership, do not dispatch a duplicate, and resume only after its
state is known. A mid-ticket restart is neither a new attempt nor a fresh
next-ticket context.

Monitor context pressure during a ticket and prepare a compact same-ticket handoff before essential state becomes difficult to retain. This supplements the required fresh context between tickets; it does not authorize an unrecorded mid-ticket reset.

## Repository-backed acceptance contract

For changes involving existing data, persisted state, or integration boundaries, ORCHESTRATOR must establish a compact acceptance contract from repository evidence before dispatching CODING work. Reuse existing evidence and record references and outcomes in the existing ticket or six-line handoff. Inspect only the relevant paths; no separate document, broad audit, agent, or approval stage is required.

Identify:

- The actual reader, writer, validator, and relevant calling path, with repository references. If one is absent, record that fact rather than inventing it.
- Representative supported input shapes and identifier formats, including how references resolve to separately stored records.
- Compatible behavior and evidence that must survive, including preserved history and newer evidence where applicable.
- Existing tests or validated fixtures that demonstrate these contracts, and any uncovered cases.
- Exact acceptance checks and expected outcomes through a relevant existing caller or boundary, including failure and rollback behavior where relevant.

ORCHESTRATOR resolves documented repository behavior directly. Missing or conflicting architectural requirements go to ARCHITECT with the evidence and one recommended resolution; hold affected coding work rather than invent a contract or ask the human to resolve routine engineering details.

Reuse existing validated fixtures where possible. Otherwise ORCHESTRATOR identifies the required shapes and checks, and CODING creates minimal synthetic fixtures derived from repository schemas, validators, and callers within the same bounded coding assignment. CODING alone writes implementation and tests. Never copy production secrets or private data. ORCHESTRATOR verifies fixture representativeness against those sources during ordinary review before relying on the results as acceptance evidence, without a separate approval ceremony.

Review must verify that fixtures match the supported input contract, checks exercise the changed behavior through a relevant existing caller or boundary, and expected results follow from requirements and repository evidence rather than mirroring the implementation. Preservation and rollback claims must cover newer evidence where applicable, not just restore an older snapshot. Passing tests based on unsupported assumptions do not establish acceptance. Report missing acceptance evidence as incomplete, not passed.

This work uses the existing ticket, ownership, review, and candidate-integrity rules. It creates no additional attempt or return allowance and never resets counters. Any failed or incomplete submitted attempt follows the existing failure lifecycle.

## Durable execution record

ORCHESTRATOR owns one durable record per ticket. Use the native tracker record when it
exists or one local file per ticket under
.scratch/<feature-slug>/issues/<NN>-<slug>.md when the tracker is unavailable.
Preserve the native dependency links and stable identity. A local draft is
valid when tracker setup or authority is absent. Do not publish comments or
close a parent issue without existing authorization.

Persist these fields before handing off and before every dispatch or review
transition:

- stable ticket identity, lineage, and parent relationship;
- spec revision and ticket revision;
- status, owner, active workers, exclusive file ownership, and dependency
  state;
- retry_policy, execution_round, round_attempts_started, attempts_started, and architect_returns_used;
- the initial base revision, candidate revision or content hash, changed-file
  list, and comparison semantics;
- the required commit identity and verified commit-tree or content digest,
  including the paths excluded because they predated this ticket;
- planned and actual model and effort;
- required checks, check results, acceptance evidence, Standards verdict,
  Spec verdict, and supervisory verdict;
- the smallest unresolved issue, remaining budget, and next action.

Use `retry_policy: two-attempt-rounds-v2`. New records start with `execution_round: 1`, `round_attempts_started: 0`, `attempts_started: 0`, and `architect_returns_used: 0`. Each round permits two attempts. Persist both attempt counters immediately before dispatch. Apply the conditional return rule in the attempt lifecycle below; do not increment a return merely because a round ended. A completed return opens the next round and resets only `round_attempts_started`. Never reset total attempts or returns. Two returns permit round 3, including attempts 5 and 6. Failure of attempt 6 exhausts the ticket without a third return or attempt 7. Missing counters must be recovered from durable history, never guessed as zero. When resuming a legacy record, migrate `astra_returns_used` to `architect_returns_used` without changing its value.

Keep candidate evidence tied to the recorded base. For a committed candidate,
record the resolved base and candidate commits or hashes. For a candidate
reviewed before commit, record the resolved starting commit, the current
working-tree candidate identity, every tracked changed file, every new file,
and the exact comparison used. A branch name alone is not immutable evidence.

At candidate submission, ORCHESTRATOR freezes all CODING writers and records a
deterministic manifest and content digest for the exact candidate. The
manifest includes every tracked and new file in the candidate and excludes
pre-existing user changes. ORCHESTRATOR rechecks that the writers remain frozen and the
same digest still matches immediately before each review verdict. Any drift
makes the review incomplete or failed and sends the evidence through the
return path. ARCHITECT rechecks the same frozen candidate digest immediately
before acceptance. A changed digest cannot receive ARCHITECT acceptance.

## Attempt lifecycle and ARCHITECT returns

An attempt is one planned CODING pass, one submitted candidate, and ORCHESTRATOR review. Normal build, test, and debugging work before submission stays inside that attempt. Multiple CODING agents collaborating on one candidate share one attempt.

After the first failed attempt in a round, ORCHESTRATOR freezes the rejected candidate, records the defects, and may dispatch a focused second attempt under the current ticket. CODING agents cannot authorize their own repair. After the second failed attempt in that round, ORCHESTRATOR freezes writers. If `architect_returns_used` is less than 2, increment it once and send the evidence plus one recommended change to ARCHITECT. Otherwise mark the ticket blocked and exhausted, with no further return or dispatch. No implementation proceeds while a return is pending.

ARCHITECT approves or revises the ticket, affected contracts, and dependent tickets. ORCHESTRATOR records the response, reconciles readiness, opens the next round, and directs CODING agents. This return may happen at most twice per ticket. After the second return, round 3 still has two attempts. If both fail, mark the ticket blocked and exhausted. Replacement workers, ticket renaming, splitting, fresh contexts, and repeated messages never reset or double-count the budget.

Successful work proceeds without using the other attempt or asking the user to approve work already authorized. Two attempts are an allowance, not an obligation to repeat known-invalid work. If an attempt exposes a missing contract, impossible requirement, or unresolved product decision, hold affected work and seek ARCHITECT clarification immediately. Clarification does not create another implementation attempt or reset the budget. A change that remedies a failed implementation remains subject to the two-return ceiling; do not disguise a failure revision as clarification. If ARCHITECT changes a requirement or dependency, hold affected downstream tickets, update their revisions and contracts, and preserve evidence that remains valid.

## Six-line handoff

Every CODING handoff from ORCHESTRATOR has exactly these six labels, in this order:
Goal, Current state, Relevant files, Constraints, Done when, Checks to run. Do
not add a seventh label. Put authority and permissions inside Constraints.
ARCHITECT uses the same six-label structure when handing work to ORCHESTRATOR.

Goal

State the one ticket outcome, stable ticket identity, spec and ticket revision,
and the assigned role.

Current state

State the actual model and effort, attempt number, attempts_started,
architect_returns_used, remaining attempt and return budget, accepted evidence,
repository base, candidate state and digest-freeze status, dependency
readiness, active workers, and the actual ORCHESTRATOR supervisor identity and
runtime-bound handle supplied by the current invocation. The handoff must bind
that handle in Current state or Constraints.

Relevant files

List the ticket, spec, ADR, review or tracker records, and the exact files the
worker owns. State whether ownership is exclusive or which worktree isolates
it.

Constraints

State the expected behavior, interface and contract boundaries, scope
exclusions, dependency holds, planned worker boundaries, permitted edits, and
authority. State that CODING alone may change implementation files and that ORCHESTRATOR
supervises this ticket. A peer cannot grant scope, ownership, or contract
changes. A new contract or dependency needs ARCHITECT's approval.

Done when

State each objective acceptance criterion, the evidence required for it, the
completion message destination, and the required candidate and review record.
State that a worker message or green checks alone is insufficient.

Checks to run

List the exact repository checks, focused behavior checks, full required suite,
diff or candidate capture, and review evidence. If a command may vary with the
current repository, state the permitted resolution method before dispatch.

ORCHESTRATOR dispatches a new or existing CODING worker through the supported spawn or
same-ticket follow-up control, with the actual model and effort selected by the
runtime. The handoff binds the actual ORCHESTRATOR supervisor handle in Current state or
Constraints. CODING acknowledges ownership before editing and sends progress,
interface proposals, blockers, and completion evidence through supported
collaboration.send_message to that bound handle. A peer or app-thread delivery
does not replace that route. ARCHITECT's handoff to ORCHESTRATOR includes the same ticket,
revision, authority, evidence, budget, and check fields inside these six
labels.

Give each worker only its assignment, relevant contracts, owned files, and required checks. Use a fresh bounded worker context by default. Inherit the full parent transcript only when specific needed information cannot be supplied in the handoff.

## Concurrent workers and communication

Multiple CODING workers may work concurrently inside one active ticket only when
their outcomes are independent. Bound concurrency to the available runtime
slots. Give each worker exclusive files or a separate worktree. Serialize
shared-file edits and integration.

If independent workers discover a shared schema or another interface they both
need, each stops before changing the shared file and sends a concrete interface
proposal to ORCHESTRATOR. Peer communication is allowed when ORCHESTRATOR is copied or informed
promptly, but a peer's agreement cannot grant scope or ownership. ORCHESTRATOR
reconciles the proposals against the recorded ticket and contracts, escalates
any ticket, dependency, or contract change to ARCHITECT, and directs a CODING
integrator for the serialized shared edit. ORCHESTRATOR then verifies the combined
candidate and records the integration evidence.

Workers acknowledge their ownership and send progress, proposals, blockers,
and completion evidence to ORCHESTRATOR. Silence, a peer's approval, or a passing local
check is not a completion signal. Do not clear the ticket context while any
current worker is writing.

Prefer completion events or blocking waits while workers run. Do useful independent work when available; otherwise wait. Avoid repeated status polling, unchanged progress narration, and rereading the same output. Investigate only a meaningful result, blocker, or evidence of a stalled worker.

## Acceptance and completion

CODING is done when the owned implementation and its evidence are submitted
within the current ticket, with the candidate/base identity and changed-file
list recorded. CODING does not close the ticket or declare supervisory
acceptance.

ORCHESTRATOR is done when it independently verifies accuracy, repository standards and
compliance, completeness, integration, every acceptance criterion, and the
required check results. ORCHESTRATOR records separate Standards and Spec verdicts,
candidate/base evidence, attempt counters, dependency state, and next action.
ORCHESTRATOR may use bounded read-only inspections, but owns the verdict and verifies
their evidence. ORCHESTRATOR submits that record to ARCHITECT and does not record successful
completion until ARCHITECT accepts the exact frozen candidate.

ARCHITECT is done when the recorded ORCHESTRATOR evidence satisfies the current spec,
ticket, and contracts. ARCHITECT may revise planning and review records and
affected contracts. ARCHITECT checks conformity with architectural decisions and unresolved contract issues. ORCHESTRATOR verifies the implementation and its evidence. ARCHITECT does not repeat the full code review or take over implementation coding or conflict resolution. Review depth follows the consequences of a defect; one review with no findings is not evidence that future review is unnecessary.

Keep the original local-commit behavior unless the user or repository
instructions override it. After ARCHITECT accepts the exact frozen candidate, ORCHESTRATOR
assigns the required local commit to a CODING integrator. The integrator stages
only candidate-owned paths,
including owned new files, and excludes pre-existing user changes. The
integrator verifies the commit identity, changed-file list, and commit-tree
content digest against the frozen candidate and records the result. If the
commit cannot be isolated safely, ORCHESTRATOR records a blocker and does not complete
or shut down the ticket.

Only after ARCHITECT acceptance, verified CODING-integrator commit, and all other
completion conditions does ORCHESTRATOR record the ticket complete, persist the
next-ticket handoff, end that ticket's workers, and make the next frontier
eligible. Keep validation evidence separate from claims of production or
end-to-end execution.

## Delegation feedback

When delegation causes substantial rework, delay, or unnecessary agent use, give a brief exception report: requested versus confirmed routing, the observed problem, and one concrete adjustment. Use measured time or usage when available and label unavailable measurements honestly. Include coordination, review, and rework when assessing efficiency. Routine small tasks need no delegation retrospective. A report does not create persistent learning; do not automatically rewrite policy from one outcome or an agent self-assessment.
