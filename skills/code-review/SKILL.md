---
name: code-review
description: Review an exact candidate against its spec and standards
---

# Code review

ORCHESTRATOR owns this review and records separate Standards and Spec verdicts. ARCHITECT owns requirements, architecture, and ticket contracts. CODING agents own implementation changes. These are model-neutral titles. Apply the current runtime profile from the [shared agent workflow](../implement/references/agent-workflow.md). This review is read-only.

Read and apply the detailed attempt, durable-record, handoff, communication,
concurrency, restart, candidate-integrity, commit, and
supervisory-acceptance rules in the [shared agent
workflow](../implement/references/agent-workflow.md) for every
implement-invoked or standalone review. Here, an approved or accepted spec
means that its current record carries product or architectural authority. This
skill remains usable with only the supplied repository, candidate, ticket, and
spec inputs.

## Authority and review input

This review is read-only. ORCHESTRATOR and any bounded read-only inspectors may inspect
the repository, candidate, tests, configuration, and records and may run
checks. They must not edit implementation code, tests, scripts, runtime
configuration, generated code, or conflicts. A fix returns through the
implementation attempt and ARCHITECT review path. A standalone review reports
findings and does not launch unsolicited edits.

The caller supplies a fixed starting point and the candidate. The starting
point may be a commit, branch, tag, or merge-base expression, but ORCHESTRATOR first
resolves it to an immutable revision and records that resolved value. Record
the starting base, candidate revision or content hash, every changed file,
and the exact comparison semantics. A branch name alone is not immutable
review input.

For a committed candidate, record the candidate commit or hash and the chosen
comparison semantics. For a review invoked by implement before commit, inspect
the exact working-tree candidate: include modified tracked files, staged and
unstaged content, and every new untracked file. Record the candidate identity
or content digest and the complete changed-file list. Do not reduce a dirty
candidate to HEAD merely because HEAD still equals the starting base.

At review submission, ORCHESTRATOR freezes all CODING candidate writers and captures a
deterministic manifest and content digest. Recheck that the writers remain
frozen and that the same digest still matches immediately before each Standards
and Spec verdict. If the candidate changes, mark the review incomplete or
failed, preserve the drift evidence, and route it through the attempt and
ARCHITECT review path. ARCHITECT must recheck that same digest before acceptance.

If the starting point cannot resolve, report an invalid-base blocked or
incomplete review and do not invent a diff. If the candidate is empty, report
an empty-diff incomplete result and do not call it a pass. Preserve these
outcomes in the review record.

## Spec and acceptance source

Use the ticket and spec revision supplied by implement or by the standalone
caller. If a local ticket, configured tracker record, or spec path is already
available, use it without asking the user to repeat it. Read the acceptance
criteria, contracts, exclusions, dependency state, and required check
evidence. If no originating spec can be found, report Spec as unavailable,
state the missing input, and do not claim an overall pass. Missing acceptance
evidence is incomplete or blocked, never a pass.

Do not require an unrelated setup skill, a particular issue tracker, or a
nonexistent command to perform a review. Ask only for a genuinely missing
fixed point, spec, or new external authority after available independent
inspection is complete.

## Standards axis

Find the repository's documented standards, such as contribution or coding
guides, and cite the file and rule for every hard standards finding. A
documented repository standard takes precedence over the smell baseline below.
Each smell is a labelled judgement call, never a hard violation. Skip smells
that repository tooling already enforces.

Apply these smell heuristics to the exact candidate:

- Mysterious Name: a function, variable, or type name does not reveal what it
  does or holds. Suggest a clearer name.
- Duplicated Code: the same logic shape appears in more than one hunk or file.
  Suggest one shared implementation.
- Feature Envy: a method reaches into another object's data more than its own.
  Suggest moving it onto the data it uses.
- Data Clumps: the same small group of fields or parameters travels together.
  Suggest a type that carries the group.
- Primitive Obsession: a primitive or string stands in for a domain concept
  that deserves its own type.
- Repeated Switches: the same switch or conditional cascade on one type
  recurs. Suggest one shared map or polymorphic implementation.
- Shotgun Surgery: one logical change forces scattered edits across the
  candidate. Suggest gathering the changing behavior in one module.
- Divergent Change: one file or module changes for several unrelated reasons.
  Suggest separating those reasons.
- Speculative Generality: an abstraction, parameter, or hook has no need in
  the current spec. Suggest deleting it until a real need exists.
- Message Chains: a caller navigates a long chain of objects. Suggest hiding
  the walk behind a method on the first object.
- Middle Man: a class or function mostly delegates to another target. Suggest
  calling the target directly.
- Refused Bequest: an implementer ignores or overrides most inherited
  behavior. Suggest composition instead of that inheritance.

## Review both axes

ORCHESTRATOR runs independent Standards and Spec inspections through the supported
collaboration runtime. ORCHESTRATOR may delegate bounded read-only inspections to CODING.
Run the axes concurrently when runtime slots permit, or sequentially when
they do not. Each inspector receives the same immutable candidate and base
evidence and a bounded, read-only assignment. ORCHESTRATOR remains responsible for
checking the work and owns both verdicts. Do not put a model selection in
skill metadata or rely on obsolete tool syntax.

The Standards report states, per file or hunk where relevant:

- every documented-standard violation, with its standards source and rule;
- every smell judgement call, with its name and candidate hunk;
- whether each finding is a hard documented violation or a judgement call;
- any tooling-enforced item that was skipped.

The Spec report states:

- each missing or partial requirement, quoting its requirement or acceptance
  line;
- each behavior outside the recorded scope;
- each requirement that appears present but is implemented incorrectly, with
  the relevant spec line and candidate evidence.

Keep the axes separate. Use these output headings on their own lines:

~~~markdown
## Standards

## Spec
~~~

Preserve the findings rather than reranking one axis against the other. End
with one line containing the finding count and the worst issue within each
axis, when either has a finding.

## Verdict and next action

Standards and Spec pass only when their evidence supports the exact frozen candidate. Overall review passes only when both axes pass, required checks and acceptance evidence are present, candidate identity still matches, and integration is verified. ORCHESTRATOR checks all delegated review work and owns the verdict.

For an implement-invoked failure, return findings to ORCHESTRATOR. After the first failed attempt in a round, ORCHESTRATOR may direct one focused CODING repair. After the second failed attempt, ORCHESTRATOR sends the evidence and recommendation to ARCHITECT if a return remains. The second ARCHITECT return opens the final round; failure of attempt 6 exhausts the ticket. Review never edits or commits the candidate and never increments a counter twice.

A standalone review reports findings and does not dispatch edits or invent retry counters. Successful review proceeds through ORCHESTRATOR verification, ARCHITECT acceptance of the same frozen candidate, and CODING integrator commit verification.
