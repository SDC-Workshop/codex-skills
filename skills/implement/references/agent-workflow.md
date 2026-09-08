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

Astra, model gpt-6-astra, is the architect and senior developer. Astra owns
requirements, architecture, specifications, ticket decomposition, acceptance
contracts, and corrections to an affected spec or dependent ticket. Preserve
Astra's configured effort. A role name in a prompt does not select a model.

Sol, model gpt-5.6-sol at medium effort, orchestrates and reviews. Sol directs
the Luna workers for the active ticket, verifies their actual changes and
checks, owns ticket status and the durable execution record, and submits
acceptance evidence to Astra. Sol keeps separate Standards and Spec review
verdicts.

Luna, model gpt-5.6-luna, is the only implementation author. Sol selects
medium, high, xhigh, or max effort for a bounded Luna assignment. Medium is the
default. Raise effort only when the assignment warrants it, and split an
assignment that remains too large.

Select these identities and efforts through the supported collaboration runtime
controls. Do not put a model field in skill metadata, silently substitute a
different model, or expand a worker's authority when a requested identity is
unavailable. Record the actual model and effort used in each handoff and in the
execution record.

Luna may author or modify implementation code, tests, scripts, runtime
configuration, generated code, and integration conflict resolutions within the
current ticket with recorded authority and its exclusive ownership. Astra and
Sol may inspect those files and run read-only checks. They may edit planning or
review records and
documentation-only planning material, but they must not author, apply, or fix
implementation changes, tests, scripts, runtime configuration, generated code,
or conflicts. A required implementation fix returns to Luna through the
attempt and Astra review path.

## Ticket and context boundary

Run one active ticket at a time. A ticket must have one verifiable outcome, a
current spec and ticket revision with recorded product and architectural
authority, explicit expected behavior and exclusions, contracts, dependencies,
worker boundaries, acceptance criteria, and exact checks or a permitted way to
resolve current repository commands at dispatch.
Resolve architecture and dependency implications before dispatch. The Sol
context for that ticket is fresh, and all worker handoffs fit inside it.

When a ticket is ready, recover its durable record before dispatch. Work starts
only when its dependencies are satisfied and the record says the ticket is
eligible. Sol can narrow an already recorded worker assignment inside the
ticket. Astra approves a ticket, contract, or dependency change.

After a ticket passes implementation, code review, Sol's supervisory
acceptance, Astra's acceptance, required commit verification, and completion
recording, persist a handoff and end that ticket's workers before starting
implementation on the next eligible ticket. Astra's acceptance must precede
successful completion recording, worker shutdown, and the next-ticket
transition. Do the same durable transition when a ticket becomes blocked or
exhausted, but do not call that transition successful completion. Carry only
the next ticket, the spec and ADR references, compact dependency and budget
state, repository revision, and relevant accepted evidence into the next
context. Do not run two tickets concurrently or clear the context while a
worker is still writing the current ticket.

Use a supported context reset or a new isolated Sol invocation with no
inherited transcript for the next ticket. The collaboration runtime supports
fork_turns: "none" with an explicit model and effort. Reusing Sol with a
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

## Durable execution record

Sol owns one durable record per ticket. Use the native tracker record when it
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
- attempts_started and astra_returns_used;
- the initial base revision, candidate revision or content hash, changed-file
  list, and comparison semantics;
- the required commit identity and verified commit-tree or content digest,
  including the paths excluded because they predated this ticket;
- planned and actual model and effort;
- required checks, check results, acceptance evidence, Standards verdict,
  Spec verdict, and supervisory verdict;
- the smallest unresolved issue, remaining budget, and next action.

Increment the relevant counter before the transition it describes. Set
attempts_started only immediately before dispatching the initial or returned
implementation attempt. When Sol records a failed attempt, first check the
budget. If another return is available, increment astra_returns_used
immediately on entry to the Astra return review cycle, persist the failure evidence
and next action, and then send the recommendation to Astra. Do not delay that
increment until redispatch. If attempts_started is already 3 or
astra_returns_used is already 2, enter blocked and exhausted status directly,
without creating a third return cycle. If state is missing, recover it from
durable history, previous handoffs, tracker events, or repository evidence.
Never guess a missing counter as zero.

Keep candidate evidence tied to the recorded base. For a committed candidate,
record the resolved base and candidate commits or hashes. For a candidate
reviewed before commit, record the resolved starting commit, the current
working-tree candidate identity, every tracked changed file, every new file,
and the exact comparison used. A branch name alone is not immutable evidence.

At candidate submission, Sol freezes all Luna writers and records a
deterministic manifest and content digest for the exact candidate. The
manifest includes every tracked and new file in the candidate and excludes
pre-existing user changes. Sol rechecks that the writers remain frozen and the
same digest still matches immediately before each review verdict. Any drift
makes the review incomplete or failed and sends the evidence through the
return path. Astra rechecks the same frozen candidate digest immediately
before acceptance. A changed digest cannot receive Astra acceptance.

## Attempt lifecycle and Astra returns

An attempt is one planned Luna coding pass, one submitted candidate, and Sol's
required review. Normal build, test, and debugging iterations before
submission are part of that attempt. A completion message or green test suite
does not close it.

The first dispatch starts the initial attempt. After Luna submits the candidate,
Sol independently checks the implementation, required checks, contracts,
integration, and acceptance evidence. If Sol returns a failed verdict or the
attempt cannot complete, Sol checks the remaining return budget before sending
anything to Astra. If attempts_started is already 3 or astra_returns_used is
already 2, Sol marks the ticket blocked and exhausted directly, preserves the
smallest unresolved issue, blocks dependents, and creates no third return
cycle. A Luna offer to patch immediately does not authorize a patch.
Otherwise Sol increments astra_returns_used, persists the failure evidence,
changes the record to waiting for Astra, and sends one concrete
recommendation to Astra. That increment records entry to the Astra return
review cycle.
An acceptance or compliance failure includes a candidate that changes a file
outside the recorded Luna ownership. Sol records that violation and routes it
to Astra; Sol and Astra do not repair the file themselves.

Each ticket allows one initial attempt and at most two Astra return cycles, for
at most three attempts total. A return cycle contains all of the following:

1. Sol records the failure evidence and proposed change after incrementing
   astra_returns_used for entry to the Astra return review cycle.
2. Astra reviews that evidence and approves or modifies the ticket, affected
   dependent tickets, and spec or contract revision.
3. Sol reconciles readiness, writes the same-ticket handoff, and increments
   attempts_started only immediately before re-dispatching Luna.

For example, a first failure leaves attempts_started at 1 and increments
astra_returns_used to 1 before Astra reviews it. After Astra approves that
return, Sol records attempts_started at 2 and astra_returns_used at 1 before
dispatching the second attempt. If that second attempt fails, the second
return is counted before Astra reviews it. The third attempt then starts with
attempts_started at 3 and astra_returns_used at 2; a third failure goes
directly to blocked and exhausted.

Successful completion follows ordinary supervisory acceptance and consumes no
failure-return cycle. If a supervisor rejects completion evidence, use the
same failure path. A replacement worker, fresh context, renamed ticket, or
split ticket keeps the original identity and lineage and cannot reset its
attempt or return budget.

After the third failed attempt, mark the ticket blocked and exhausted. Preserve
all attempts, checks, review verdicts, acceptance gaps, and the smallest
unresolved issue. Block every dependent ticket. Continue only with independently
eligible frontier tickets, each in a fresh ticket context. Do not accept broken
work silently and do not ask for user approval for a routine successful
ticket. A retry after exhaustion requires new bounded authorization.

When Astra changes a requirement, output contract, dependency, or acceptance
boundary, increment the affected spec or ticket revision. Put affected
downstream tickets on hold, reconcile their contracts and readiness, and
invalidate acceptance evidence that depends on the old revision before release.
Preserve accepted evidence that does not depend on the changed contract. Sol
records which evidence survived, which became stale, and the next eligible
frontier.

## Six-line handoff

Every Luna handoff from Sol has exactly these six labels, in this order:
Goal, Current state, Relevant files, Constraints, Done when, Checks to run. Do
not add a seventh label. Put authority and permissions inside Constraints.
Astra uses the same six-label structure when handing work to Sol.

Goal

State the one ticket outcome, stable ticket identity, spec and ticket revision,
and the assigned role.

Current state

State the actual model and effort, attempt number, attempts_started,
astra_returns_used, remaining attempt and return budget, accepted evidence,
repository base, candidate state and digest-freeze status, dependency
readiness, active workers, and the actual Sol supervisor identity and
runtime-bound handle supplied by the current invocation. The handoff must bind
that handle in Current state or Constraints.

Relevant files

List the ticket, spec, ADR, review or tracker records, and the exact files the
worker owns. State whether ownership is exclusive or which worktree isolates
it.

Constraints

State the expected behavior, interface and contract boundaries, scope
exclusions, dependency holds, planned worker boundaries, permitted edits, and
authority. State that Luna alone may change implementation files and that Sol
supervises this ticket. A peer cannot grant scope, ownership, or contract
changes. A new contract or dependency needs Astra's approval.

Done when

State each objective acceptance criterion, the evidence required for it, the
completion message destination, and the required candidate and review record.
State that a worker message or green checks alone is insufficient.

Checks to run

List the exact repository checks, focused behavior checks, full required suite,
diff or candidate capture, and review evidence. If a command may vary with the
current repository, state the permitted resolution method before dispatch.

Sol dispatches a new or existing Luna worker through the supported spawn or
same-ticket follow-up control, with the actual model and effort selected by the
runtime. The handoff binds the actual Sol supervisor handle in Current state or
Constraints. Luna acknowledges ownership before editing and sends progress,
interface proposals, blockers, and completion evidence through supported
collaboration.send_message to that bound handle. A peer or app-thread delivery
does not replace that route. Astra's handoff to Sol includes the same ticket,
revision, authority, evidence, budget, and check fields inside these six
labels.

## Concurrent workers and communication

Multiple Luna workers may work concurrently inside one active ticket only when
their outcomes are independent. Bound concurrency to the available runtime
slots. Give each worker exclusive files or a separate worktree. Serialize
shared-file edits and integration.

If independent workers discover a shared schema or another interface they both
need, each stops before changing the shared file and sends a concrete interface
proposal to Sol. Peer communication is allowed when Sol is copied or informed
promptly, but a peer's agreement cannot grant scope or ownership. Sol
reconciles the proposals against the recorded ticket and contracts, escalates
any ticket, dependency, or contract change to Astra, and directs a Luna
integrator for the serialized shared edit. Sol then verifies the combined
candidate and records the integration evidence.

Workers acknowledge their ownership and send progress, proposals, blockers,
and completion evidence to Sol. Silence, a peer's approval, or a passing local
check is not a completion signal. Do not clear the ticket context while any
current worker is writing.

## Acceptance and completion

Luna is done when the owned implementation and its evidence are submitted
within the current ticket, with the candidate/base identity and changed-file
list recorded. Luna does not close the ticket or declare supervisory
acceptance.

Sol is done when it independently verifies accuracy, repository standards and
compliance, completeness, integration, every acceptance criterion, and the
required check results. Sol records separate Standards and Spec verdicts,
candidate/base evidence, attempt counters, dependency state, and next action.
Sol may use bounded read-only inspections, but owns the verdict and verifies
their evidence. Sol submits that record to Astra and does not record successful
completion until Astra accepts the exact frozen candidate.

Astra is done when the recorded Sol evidence satisfies the current spec,
ticket, and contracts. Astra may revise planning and review records and
affected contracts. Astra does not take over implementation coding, conflict
resolution, or an open-ended duplicate audit.

Keep the original local-commit behavior unless the user or repository
instructions override it. After Astra accepts the exact frozen candidate, Sol
assigns the required local commit to a Luna integrator. The integrator stages
only candidate-owned paths,
including owned new files, and excludes pre-existing user changes. The
integrator verifies the commit identity, changed-file list, and commit-tree
content digest against the frozen candidate and records the result. If the
commit cannot be isolated safely, Sol records a blocker and does not complete
or shut down the ticket.

Only after Astra acceptance, verified Luna-integrator commit, and all other
completion conditions does Sol record the ticket complete, persist the
next-ticket handoff, end that ticket's workers, and make the next frontier
eligible. Keep validation evidence separate from claims of production or
end-to-end execution.
