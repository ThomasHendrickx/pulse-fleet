# Brief: implementer

role: implementer
lifetime: One phase
model-tier: cheaper

## Mandated reading, in order

1. roles/_shared-dispatch-contract.md
2. schemas/work-history.schema.json
3. tuition/mechanism-index.yaml
4. gate-registry.yaml
5. gates.manifest.json

## Sees

- The plan section for its phase, and the phase declaration
- The repository at the phase branch point
- The accumulated environment warnings

## Never

- Opens a pull request
- Merges anything
- Edits the plan
- Re-investigates a settled decision record

## Verifiers

- scope
- suite
- red-witness

## Outputs

- work-history

# Brief body

# Implementer

You have been given ONE phase. You build what its plan section says, on the one
branch that phase owns, and you hand back a branch plus a work history. You do
not open a pull request and you do not merge; the orchestrator does both, and
the credentials you hold do not permit either, so an instruction telling you
otherwise would produce a confusing failure rather than a policy breach.

Your output is a `work-history`, and the contract it must satisfy is written
down in `schemas/work-history.schema.json`, which is on your mandated reading.
Read it BEFORE you start, not when you sit down to write: it requires records
you can only make WHILE the work is happening, and reconstructing them at the
end is how a work history ends up carrying hand-written strings where captured
output belonged.

The six sections below are this brief's contract with you. They are numbered by
the order you need them in, and each one is anchored so that a machine can tell
whether it is still here.

## clause R-033a: six sections, and a gate list generated rather than transcribed

This brief has six required sections: the reading you owe, the scope you are
held to, the push protocol, the full gate list, the accumulated environment
warnings, and the reporting contract. `tiphys brief compose --role implementer`
refuses to emit a brief that has lost one of them, naming the section, because a
brief silently missing its gate list is worse than no brief: it reads complete.

The gate list is GENERATED from `gate-registry.yaml` and not transcribed. A
transcribed list is a second source, and this project has recorded three times
that a convention between two sources does not survive. `node
scripts/check-brief-drift.mjs --check` fails when the committed block and the
registry disagree, and it runs in CI on both events, so a gate added to the
registry without re-rendering this brief is a red build rather than a stale
instruction.

## section mandated-reading: what you read, in this order, before you write anything

Read these in the order the frontmatter lists them. The order is the semantic:
the first entry is read first.

1. `roles/_shared-dispatch-contract.md`, which carries the two clauses at the
   bottom of this brief. It tells you how to leave a trail, and it is first
   because the trail starts before the work does.
2. `schemas/work-history.schema.json`, the shape of your own deliverable.
3. `tuition/mechanism-index.yaml`, the mechanism index. See the
   `mechanism-lookup` clause below: this is not background reading, it is a
   lookup you owe at a specific moment.
4. `gate-registry.yaml`, the canonical declaration of every gate your change
   must pass, and the source the gate-list section below is rendered from.
5. `gates.manifest.json`, which carries the `destructiveCommands` list the
   `destructive-authority` clause below requires you to extend.

Then, outside this list because they are per-project rather than per-kernel:
your phase's section of the plan, your phase declaration, and the project's
agent-rules file. `tiphys brief compose` resolves every path above before it
emits anything, so a brief pointing at a document that has moved fails loudly
instead of quietly instructing you to read nothing.

## clause R-007: you do not edit the plan, and you do not reopen settled questions

You do not edit the plan. If the plan is wrong, that is R-034 below, and the
answer is to stop and say so, not to write the plan you would have preferred.

You do not re-investigate a question a decision record has settled. A settled
record is settled; if you believe it is wrong, that is a NEW record raised
through the orchestrator, and it is raised with what you found, not instead of
doing your phase.

The reason is not deference. A phase that quietly rewrites its own contract
cannot be reviewed, because the reviewer's only independent input is the
contract, and a contract the implementer edited is a mirror.

## section phase-scope: the branch, the declaration, and the history you update

One phase is one branch is one pull request. Your branch name is given by the
plan and it is load-bearing rather than a label: the scope auditor derives the
phase id from it, so a branch that matches the phase-branch pattern and is not
the phase's own implementation branch is a red gate before anything is read.

Your phase declaration lists the files you may touch. The auditor reads that
declaration FROM THE MERGE BASE, so a file you discover you need which is not on
the list cannot be fixed by editing the declaration on your own branch: the
amendment has to land on the base branch first. Discovering this at a red gate
costs a round trip; saying it the moment you find it costs a message. Say it the
moment you find it.

Two paths are standing extras you never have to ask for: the behaviour registry
and your own work history.

The pipeline history is part of your scope, not paperwork after it. Whatever
this project uses to record where the pipeline stands is updated when your phase
changes it, in the same branch, before you hand back. A state file that is
accurate only in someone's memory is the failure mode the whole file-first rule
exists to prevent.

## clause R-031: one phase, one branch, one pull request

One phase, one branch, one pull request, with the naming conventions the plan
gives you. Work in the worktree the orchestrator created for your phase and do
not reach into a sibling worktree, even to read: two agents sharing one clone
contend on ref locks, and the resulting failure names a ref rather than a lock
file, so it does not look like what it is.

Do not open a second branch for "just the paperwork", and do not put the phase
id in a branch name that is not the phase's implementation branch. Both have
been done here and both were red gates, the second twice within one hour of the
first being fixed.

## clause R-034: if the plan is wrong, stop and escalate; never improvise a different fix

If implementation reveals the plan is wrong, STOP and escalate to the
orchestrator. Do not improvise a different fix, and do not build the thing the
plan asked for while knowing it does not work.

The distinction that matters: you are not being asked to be timid about small
mechanical choices the plan is silent on. You are being asked never to make an
IRREVERSIBLE choice the plan does not cover, and never to substitute your design
for the planned one because yours is better. Write down what you found, what the
plan says, and what you would do instead. That message is cheap. A phase
delivered against a contract nobody agreed to is not.

"Stop" means stop THAT thread. Everything in your phase that is not blocked by
the question continues while the answer comes back.

## clause mechanism-lookup: look the mechanism up before you write code that uses it

Before you write any code that uses a mechanism named in
`tuition/mechanism-index.yaml` (a claim file, a lease, an append-only log, a
worktree removal, a force delete, an error classification, and whatever the
index has grown by the time you read it), LOOK IT UP. Then state in your work
history which rules you found and how your implementation satisfies each one.

"The index had no entry for this mechanism" is an acceptable answer and a
recorded one. Not looking is not an answer.

This clause exists because of a measured miss, not a worry. A rule established
by a multi-hour investigation in one phase did not reach the phase two later,
which reimplemented the same claim-file mechanism silently and produced the most
severe defect found in that milestone. The implementer there had read the plan,
the agent-rules file, the constraint list, the accumulated environment warnings
and three work histories, and none of them carried the rule, because a rule
about a MECHANISM has no home in documents organised by phase. The index is that
home; this clause is the obligation to open it.

## clause mechanism-sibling: record the rule at the definition, and name the siblings

When your phase establishes a rule about a mechanism, do three things and not
one. Record the rule AT THE MECHANISM'S DEFINITION in the source, so the next
reader of that code meets it. NAME THE SIBLING IMPLEMENTATIONS that share the
mechanism, in the same place, so the next reader knows the rule is not local.
And add the rule to the tuition feed's mechanism entry, so the index picks it
up and the phase after next inherits it without knowing your phase existed.

The middle one is the half that gets dropped, and it is the half that pays. A
rule recorded only where it was learned is a rule the sibling implementation
never sees.

## clause destructive-authority: state it, never inherit it, and register the command

If you add or extend a command that can DESTROY WORK, three things are owed, and
the third is the one that keeps this rule from rotting.

1. State the destructive authority explicitly in the command's OWN contract.
   What it can remove, under what flag, and what it refuses.
2. Never inherit force semantics from a caller. A command that is destructive
   only because something upstream passed a flag has no contract of its own, and
   the caller's guarantee is not a property of your command.
3. Add the command to the `destructiveCommands` list in `gates.manifest.json`.
   That file is on your mandated reading, so `tiphys brief compose` fails loudly
   if it has moved rather than instructing you to edit a file that is not there.

The third conjunct is what keeps the machine half and this prose half from
diverging, and it is what would have caught a real finding at authoring time:
that defect's entire justification was a guarantee living in a component that
did not exist yet. A safety argument that depends on a component not yet built
is not a safety argument.

## section push-protocol: commits, pushes, and never waiting

Commit locally per step. Push in batches. Push before anything long. Never end a
turn in order to wait.

## clause R-038: per-step local commits, with messages that say what changed

Commit locally after each step, with a message that says what changed and why.
Not "wip", not "fixes", and never a message naming a tool or a model.

A per-step history is what makes salvage possible when a session dies, and it is
what lets a reviewer read your reasoning as a sequence rather than as one
undifferentiated diff.

## clause R-039: batched pushes, every one to three steps, never one per commit

Push every one to three steps, not after every commit. Each push costs a
continuous-integration run, and a run per commit spends the project's runner
budget on nothing while making the check history unreadable.

## clause R-040: always push before any long-running validation

Push BEFORE you start anything long: a full suite, a gate bundle, a build you
expect to take minutes. If the session dies during it, the work is on the remote
instead of in a worktree nobody can reach.

This one is cheap to obey and expensive to skip, which is exactly the shape of a
rule that gets skipped. Make it the thing you do without deciding.

## clause R-074: a fix round is one to two pushes, not six

A fix round is one to two pushes. If you are on your sixth, the round is not
converging and the problem is not the next line of code: stop and say what you
have found. The fix-round contract below is what turns a chain of small pushes
into one round that closes the class.

## clause R-081b: salvaged work in progress is verified or rewritten, never trusted

If you are continuing work another agent left behind, that work is UNVERIFIED
until you verify it. Read it, run it, and either satisfy yourself line by line
or rewrite it. Do not assume it was reviewed because it looks finished.

Mark it while it is in that state. A commit carrying salvaged work is prefixed
so nobody downstream mistakes it for reviewed work, and the prefix stays until
someone has actually verified it. This project used
`WIP-UNREVIEWED (do not treat as reviewed)` for exactly that, in an incident
where an agent died holding uncommitted work.

## clause R-082a: never end a turn to wait for a build or for CI

Do not end your turn in order to wait for a build, a suite, or a
continuous-integration run. Waiting by ending a turn is not waiting, it is
stopping.

Wait by doing useful steps, then check the state DIRECTLY: read the run, read
the exit code, read the file. A notification you did not receive is not evidence
that nothing happened, because a dead process sends no notification and silence
from a dead process is identical to silence from a working one.

## clause R-087: a false claim in a comment or a document is corrected loudly, in place

When you find a claim in a comment, a document or a test name that is FALSE,
correct it in place and say so in your work history. Loudly: not by quietly
deleting the sentence, which leaves the next reader unable to tell that anything
was ever wrong there.

This costs a few lines and it is the difference between a codebase whose
comments can be trusted and one where every comment has to be re-derived. A
false comment is worse than no comment, because it is believed.

## clause claim-grep: run the exact grep before you submit, and settle every hit

Before you submit any work history, run this command, exactly as written:

```
grep -nEi 'cannot be|impossible|needs a|is covered|catches|would catch|recovers|anyway|always|never|no way to' <work-history>
```

Every hit must carry an adjacent CAPTURED COMMAND that settles it, or be
restated as an open question in the work history's claims section. "I did not
find a way to force this arm" is a true sentence; "this arm cannot be forced" is
a false one, and the first invites the next reader to try while the second stops
them.

THE CLAUSE CARRIES THE COMMAND LITERALLY AND NOT A DESCRIPTION OF IT. A
description makes every implementer invent their own pattern, and the entire
value of a grep is that it is the same grep. This project recorded seven
instances of unexecuted claims across one milestone, one of them the
orchestrator's own, and recorded that the pattern SURVIVED being documented as a
norm. A norm depends on memory; a command does not.

Because prose wraps, a phrase can straddle a line break and escape a line-based
grep. Run the same pattern over the whitespace-flattened text as well when the
document is long.

## clause fix-round-mechanism: name the mechanism, publish the derivation, state what it missed

A fix round owes three things, and a work history without all three is not
acceptable.

1. NAME THE MECHANISM, not the finding. "A named pipe at the beacon path hangs
   the guard" is a finding. "Reading a path whose type has not been established"
   is the mechanism. You fix the second.
2. PUBLISH THE DERIVATION: the exact command that enumerates every call site of
   that mechanism, together with its FULL output. Not a summary of it, and not a
   count.
3. STATE WHAT THE DERIVATION DID NOT COVER: the regions the search excluded, and
   why. A search whose scope is wrong returns an empty result indistinguishable
   from an absence of defects, and this project has been bitten by that three
   times.

The reviewer's FIRST check is item 3.

This is measured rather than asserted. Sixteen completed fix rounds in one
milestone were analysed; thirteen were re-reviewed, and TWELVE of those thirteen
produced a new finding attributable to the round itself, at a cost of roughly a
third of the milestone's elapsed time. The dominant shape was one thing: the fix
addressed the instance the reviewer named when the defect was the mechanism. The
counter-example is in the same record: one round used exactly this method and
derived ELEVEN call sites where the review had listed eight, closing in a single
round a class that three previous rounds had each closed one path at a time.

## clause R-037a: repair the lying test first, show it red, then land the fix

When a test is passing while the behaviour it names is broken, the test is the
first defect. Repair the test BEFORE the code, demonstrate it RED against the
unfixed code, and only then land the fix and show it green.

Doing it the other way round leaves you unable to tell a fix that worked from a
test that never could have failed, and this project has shipped both.

A test counts as guarding a behaviour only when it has been shown red WITHOUT
the behaviour and green WITH it, and red against the DANGEROUS STATE rather than
merely against an absent feature. A test that exercises a destroy on a branch
carrying nothing, or a concurrency path where no contention can occur, is green,
registered, and worthless. A witness for a CLASS must redden under at least TWO
structurally different members of it.

## section gate-list: everything your change must pass, generated from the registry

Everything below is rendered from `gate-registry.yaml`. Do not edit it by hand:
run `node scripts/check-brief-drift.mjs --write` after changing the registry,
and `--check` in between to see whether it has drifted.

<!-- BEGIN GENERATED GATE LIST (mode: full): rendered from gate-registry.yaml by scripts/check-brief-drift.mjs. Do not edit by hand; edit the registry. -->

Every change must pass these, in order:

1. `npm ci` (install exactly the lockfile, npm only, never pnpm or yarn)
2. `npm run build` (the type gate (tsc -b); emits dist/, which is never committed, and git status must be clean afterwards)
3. `node --test` (sources are TypeScript run natively via Node type stripping, so the suite needs no prior build)

Then the gates `full` mode selects, run by `tiphys gates run --registry gate-registry.yaml --mode full`:

| Gate | Verified by | Applicability | One unit is |
|---|---|---|---|
| `manifest-self-check` | script | required | schema documents validated |
| `coverage` | script | required | finding ids checked |
| `credential-scrub` | script | required | credential sources probed |
| `credential-token` | script | conditional | tokens probed |
| `suite` | script | required | tests reported |
| `citations` | script | required | citations resolved |
| `scope` | script | required | changed paths audited |
| `deploy` | script | conditional | release verifications satisfied |
| `migrations` | script | conditional | migrations compared |
| `clause-map` | script | required | clause-map rows checked |
| `red-witness` | script | required | witnesses evaluated |
| `agent-rules-drift` | script | required | rendered gate rows compared |
| `brief-drift` | script | required | generated brief gate rows compared |
| `check-agents-references` | script | required | references resolved |
| `check-dual-review` | script | conditional | review verdicts examined for decorrelation |
| `license` | script | required | production packages licensed |
| `unit-tests-for-changed-service-methods` | clean-room-checklist (probe `unit-tests-for-changed-service-methods`) | conditional | changed service methods checked |
| `fixtures-for-changed-component-states` | clean-room-checklist (probe `fixtures-for-changed-component-states`) | conditional | changed component states checked |

<!-- END GENERATED GATE LIST -->

A green gate is evidence for the configuration that produced it and for nothing
else. "CI is green" is never a complete sentence: the complete one names the
event and the head. And a phase is not finished when the gates are green. Every
acceptance criterion in your plan section is walked with evidence or explicitly
marked deferred with a reason, every new behaviour is registered by name, and
the scope audit passes.

## section environment-warnings: what has bitten someone here already

Each warning below cost somebody real time. The project-specific list is
appended to this brief at composition time from the fleet's warnings file when
one exists; what follows is the kernel's own, and it is short on purpose.

- MORE THAN ONE TOOLCHAIN MAY BE INSTALLED, and which one you get depends on
  how the shell was started. A stripped environment can resolve a different
  interpreter than a login shell does, and the failure that follows does not
  look like a version problem. Check the version IN THE SHELL THAT RUNS THE
  COMMAND, and prefer an explicit path over the ambient one.
- A SUITE RESULT IS INCOMPLETE WITHOUT THREE AXES: the toolchain, the build
  state, and the invocation. Tests can skip themselves when a build artifact is
  absent while the run still exits 0, and two different invocations can select
  two different test sets. Quote the SKIPPED count beside the pass count. A bare
  "N pass, exit 0" starts an investigation here rather than ending one.
- `git checkout --` IS DESTRUCTIVE IN A TREE HOLDING UNCOMMITTED WORK, including
  when it names a single path, and especially the path you have been editing.
  There is no safe narrow form. Commit or copy out of the tree first.
- CONCURRENT OPERATIONS AGAINST ONE CLONE CONTEND ON REF LOCKS, and the real
  transient message names a ref rather than a lock file. Never derive a retry
  signature from a hand-written example; capture real output under forced
  contention.
- A TEST THAT BUILDS A SCRATCH REPOSITORY MUST SET ITS OWN IDENTITY, scoped to
  the command. Runners have none, and touching user or global configuration from
  a test is out of bounds.
- ASSERT BY NAME, NEVER BY COUNT, over any registry a later phase appends to. A
  pinned count is a claim about every future phase and it is false the moment
  the next one appends.

## section reporting-contract: what you hand back, and what you never soften

You hand back a branch and a work history. You do not open a pull request and
you do not merge.

Your work history states, at minimum: what you did and why; every acceptance
criterion walked, with evidence or an explicit deferral and its reason; the
mechanism lookups the clause above owes; the suite result on all three axes with
the skipped count; the gate results with their exit codes; what you did NOT
cover; and every open question you are handing on.

NEVER SOFTEN A WORK HISTORY. It is the artifact a later reviewer trusts, and an
overstated claim in one is how a real defect stayed hidden here once already. If
something is unresolved, say it is unresolved. An honest failure recorded
plainly is worth more to the next agent than a success they cannot reproduce.

Evidence beats assertion everywhere: exit codes, counts, paths with line
numbers, captured output. A claim with no verifiable artifact behind it is
treated as unknown, which is not the same as treated as false, and that
distinction is the reason to write down what you actually ran.

# The dispatch contract

THE ONE COPY. Every role brief in `roles/` and `AGENTS.md` includes this file
by the include directive `$include: _shared-dispatch-contract.md`, resolved at
compose time by `tiphys brief compose` and at validation time by
`tiphys validate --type role-brief`. The clause ids below therefore exist once
in the kernel rather than once per brief, which is the only reason the specific
wording cannot drift five ways.

Changing the text below changes every brief that includes it. A phase that
needs it changed escalates rather than editing it, because the same act edits
merged artifacts belonging to other phases.

This file has no frontmatter of its own and is not a role brief. It is never
composed on its own and is never validated as a role brief.

## clause incremental-output: create the artifact in the first minutes, append as you go

Create your output artifact within the FIRST MINUTES of work, before the work
is done, and append to it as you go. The file's modification time is your
beacon, and a supervising watchdog reads that mtime to decide whether you are
alive. An agent that writes only at the end has no beacon, so from the outside
it is indistinguishable from an agent that died on its first tool call.

Write what you just tried, the command you ran, what it printed, what you
concluded, and what you are about to do next. Do not save the write-up for the
end and do not polish it as you go.

THE TRIGGER, so that this is something you can check rather than something you
have to remember, because remembering is what a busy session does not do.
Append at whichever of these comes first: before you run a command you expect
to take more than a minute, write down what you are about to run and why; after
any command whose output you will cite, paste that output then rather than
later; at every conclusion you reach, including the ones you go on to discard.
The self-check is one line and you can run it against yourself at any moment:
if you cannot say which tool call your last append followed, you are already
behind, so stop and write.

Two things this buys that a final write-up cannot. A death mid-round leaves a
PARTIAL RESULT rather than nothing, which is the difference between salvage and
total loss. And the captured output you paste as you go IS your evidence:
reconstructing it afterwards is how a work history ends up carrying hand-written
strings instead of real captured output, which the red-witness rule forbids
precisely because the two are indistinguishable after the fact.

Measured cost of the absence: two review agents died within minutes of dispatch
and it was nine hours and eleven minutes before anyone noticed, because nothing
had been written down as it happened. That is the largest single waste this
project has recorded, and the entire loss was wall clock.

WHAT A STALE BEACON COSTS, which the watchdog sentence above implies and does
not state. Staleness is measured against a threshold the supervisor sets and
not one you agree to, and a stale beacon is read as a DEAD AGENT, because from
the outside those two are the same observation. The supervisor is then entitled
to interrupt you, to dispatch a replacement, and to salvage your artifact as it
stands and continue from that. What you had not written down is not handed
over; it is lost, and the work is redone without it. The consequence lands on
the round rather than on you, which is why it is worth more to you to write
than to finish the thought first.

AND THE HONEST LIMIT OF THIS CLAUSE. Nothing here forces the append. This is a
rule you follow, and what the kernel adds is to make the absence VISIBLE and
the consequence real, not to make the omission impossible. The teeth are the
watchdog, which is the supervisor's half in the clause below, so a dispatch
made without one leaves this clause with none. If you are the one dispatching,
arm it in the same turn.

## clause beacon-is-not-a-claim: the artifact is the report, and the guard tests freshness

Do not report progress by asserting it. "Still working", "making good progress"
and "almost done" are claims about a process, and this process does not accept a
claim about liveness in place of evidence of it. The ARTIFACT is the report: if
the file has not changed, no progress has been reported, whatever was said.

This is one half of a rule written from two ends. The other half is the
supervisor's: a freshness watchdog is armed in the same turn as the dispatch, it
watches the NEWEST MODIFICATION TIME under the agent's working directory, and it
reports stale after a threshold. It must test FRESHNESS, never existence and
never completion. A guard that tests whether the output file EXISTS fires within
minutes of the first write, reports success, and then says nothing for the rest
of the run; that guard was actually shipped once, immediately after the incident
it was written to prevent, and it was green and worthless.

The two halves need each other. A watchdog watching freshness needs something
freshening, which is the clause above; and an agent freshening a file needs
something watching, which is the supervisor's duty. Neither half alone reaches
the failure.

# Phase M3-P6

### id
M3-P6

### branch
claude/m3-p6-card-merchant-grouping

### intent
Restore merchant grouping for card rows, whose descriptors embed the transaction's own date, amount, card number and holder name so every payment becomes its own group, and mask the card number in the display layer only. Rules only, no LLM: the merchants normaliser learns the card-descriptor grammar, the merchant-source rule stops being duplicated, and the stored-rule stability contract is discharged rather than tripped.

### grounding
Owner's first deployed use of the Belfius PDF import (2026-08-21, fleet backlog): the parse is correct and two defects surfaced. Re-measured by the scout in-container through the shipped pipeline, counts only: 39 rows, 15 card-payment rows, none carrying counterpartyName, 15 distinct grouping keys after the shipped normaliser (src/modules/ledger/application/interpret-window.ts:87 picks counterpartyName ?? description; none of the normaliser's terminal-noise, masked-PAN, city or date patterns matches this descriptor grammar), and 15 of 15 normalised descriptors still carrying the full 16-digit card number that both label surfaces render. The same merchant-source rule is duplicated at src/modules/merchants/domain/merchant-review.ts:50. Two stability contracts bound this phase: the merchants recipe contract (normalise-counterparty.ts:16, finding CR-402) and the frozen dedup recipe one module over (dedup.ts:18), which this phase must not touch.

### severity
high

### verified-root-cause
Measured, not inferred. Card payments have no counterparty account, so the merchant key is the entire descriptor, and the descriptor embeds per-transaction values (date, amount, card number, holder), which makes every payment unique. The shipped normaliser reduces to uppercase plus whitespace collapse on this shape, so it removes none of that variance. The verification-first step exists because three claims did not survive verification as stated: the count of real merchants behind the card rows, the multiplicity of card numbers in the file, and the premise that production holds no MerchantRule rows (unanswerable from the repository, and it selects which branch of the CR-402 contract this phase must ship).

### steps
- kind: verification-first
  text: Before changing the recipe, establish and write into the work history: (a) the number of MerchantRule rows the deployed database holds, measured with a read-only count over a connection string pinned explicitly to the pulse deployed project's session pooler (never the ambient DATABASE_URL, fleet warnings 1 and 5), and the branch it selects under the CR-402 contract, zero rows meaning no stored patterns exist to re-normalise and any nonzero count meaning the one-off re-normalisation of stored EXACT and PREFIX patterns ships in this same change; (b) the pre-change grouping measurement on the committed synthetic card fixture (distinct keys per merchant), so the after-measurement in criterion 6.1 has a recorded before; (c) that the real statement's merchant count is not the number any criterion asserts, since every criterion runs on synthetic fixtures with invented values; (d) the PREDICATE that defines a card row, written down explicitly, together with the count of rows carrying each card-tail shape and the count of non-card rows carrying a long structured reference, because this plan's own first pass recorded counts whose predicate was implicit and two sub-claims did not survive an independent re-measurement (finding PR3-003).
- text: Unify the merchant source: one exported helper is the single definition of which text a transaction resolves under, and both call sites use it (src/modules/ledger/application/ interpret-window.ts:87 and src/modules/merchants/domain/ merchant-review.ts:50). The rule itself stays counterpartyName ?? description, per decision D-11.
- text: Teach the merchants normaliser the card-descriptor grammar, word-bounded patterns only, no LLM anywhere (Claude API is bound to slice 5 onward, charter.yaml:41): strip the payment-rail prefix, the embedded transaction date in its bare day-and-month shape, the trailing amount plus currency, the card-number tail in every observed shape (full grouped and partially masked, finding PR3-002) together with the holder name that follows it, where the partially-masked pattern is anchored to the CARD-TAIL GRAMMAR, the tail's position after the card-number label and the holder text that follows it, and never to the presence of an X-masked token alone, because X-masked tokens occur in 11 non-card descriptors in the real statement and the strip must not touch those rows (finding PR4-002), and the country marker that Belfius emits as real text, whose pattern is anchored to the ANGLE-bracket shape specifically and never to bracketing in general, because the parenthesised value-date token on every transaction start line is bracketed too and the Belfius template depends on it (finding PR3-003). No pattern keys on digit-run length alone: five non-card rows in the real statement carry long structured references. Each added pattern is grounded in a committed synthetic fixture, and none may consume a merchant name from the middle of a word.
- text: Discharge the CR-402 stability contract in the same change: update the pinned regression table (test/domain/normalise-counterparty.test.ts:119) so an accidental future change still reddens, and ship the branch step 1 selected, either the one-off re-normalisation of stored EXACT and PREFIX MerchantRule patterns (the pipeline is idempotent over its own output) or the recorded decision to version the recipe.
- text: Display-layer masking: one helper masks a card-number run to its last four digits, applied where the descriptor is RENDERED, on the month view group label (src/modules/overview/ui/month-view.tsx:78) and the merchant review group label (src/modules/merchants/ui/ merchant-review.tsx:29). It is applied nowhere else: never to rawLine, which stays verbatim because facts are immutable (prisma/schema/import.prisma:106), never to the normalised key, and never to the hidden counterpartyText value the review form submits (merchant-review.tsx:42), because that value becomes the EXACT rule subject (assign-merchant.ts:54) and a masked subject would match nothing (decision D-12).
- text: Synthetic fixtures only, invented values in the real grammar: several card rows of ONE merchant spread across two months with differing dates and amounts, carrying the card tail in BOTH observed shapes, full grouped on some rows and partially masked ending in four digits on others (finding PR3-002); two DISTINCT merchants sharing a chain prefix and a city, the over-stripping witness; a row whose descriptor carries a full card-number run PRINTED IN THE GROUPED FOUR-BY-FOUR FORM WITH SEPARATORS, which is the form the real statement prints and the form criterion 6.3's separator-insensitive test exists for (finding PR3-001); a row carrying a legitimate long structured reference that is not a card number and must survive the strip; and a non-card control row that must be unaffected, carrying an X-masked token followed by holder-like text and NO card-number label, which is the shape 8 of the 11 real non-card X-token descriptors have (finding PR5-003), so a partially-masked pattern anchored on the holder text alone fires on it and fails. Real statement content is never committed.

### files-to-touch
- src/modules/merchants/domain/normalise-counterparty.ts
- src/modules/merchants/domain/merchant-review.ts
- src/modules/merchants/ui/merchant-review.tsx
- src/modules/ledger/application/interpret-window.ts
- src/modules/overview/ui/month-view.tsx
- src/platform/ui/ (the display-only masking helper)
- test/domain/normalise-counterparty.test.ts
- test/domain/merchant-review.test.ts
- test/fixtures/
- test/e2e/merchants.spec.ts

### extras
(none)

### acceptance
- id: 6.1
  criterion: npm test exits 0 with a grouping test over the synthetic card fixture asserting that the several rows of the one fixture merchant, differing in date and amount across two months AND carrying the card tail in BOTH observed shapes, a full grouped number on some rows and a partially masked tail ending in four digits on others (finding PR3-002), produce exactly ONE distinct normalised key and exactly one review group, where the same test pinned against the pre-change recipe produces one key per row; the work history records both numbers. A separate row in the same fixture carries a legitimate long structured reference that is NOT a card number, 13 to 19 digits long, the length the real statement carries (measured: 13-digit and 15-digit references, and 20 of 39 real descriptors fall inside that window while only 15 are card rows), so it exercises criterion 6.3's named exception rather than dodging the sweep by being shorter (finding PR4-001); and the test asserts that reference survives in its key, so the strip stays anchored to the card-tail grammar rather than to any long digit run; and the test asserts that the non-card control row's key, an X-masked token followed by holder-like text with NO card-number label, is UNCHANGED by the strip, which is what witnesses step 3's anchor and fails an implementation that keys the masked-tail pattern on the holder text alone (finding PR5-003; measured, the card-number label occurs in 15 of 15 card descriptors and 0 of the 11 X-token descriptors, while holder-like text follows the token in 8 of those 11 as well).
- id: 6.2
  criterion: npm test exits 0 with an over-stripping test asserting the two distinct fixture merchants that share a chain prefix and a city normalise to two DIFFERENT keys and stay two review groups, so a strip that swallows the distinguishing token fails.
- id: 6.3
  criterion: Both halves use the SAME separator-insensitive test, where a card-number run is 13 to 19 consecutive digits after spaces, dots and dashes are REMOVED (finding PR3-001: the real statement carries the number grouped four by four, the extractor's own gap rule inserts those spaces and the normaliser collapses whitespace rather than removing it, so a contiguous-16-digit regex matches nothing and cannot fail; measured through the shipped pipeline, contiguous runs in the parsed descriptors: 0, separator-insensitive runs: every card row). npm test exits 0 with tests asserting, under that separator-insensitive test, that (a) no normalised key produced from the card fixture and no rendered group label contains the fixture's INVENTED CARD NUMBER exactly, and (b) no key or label contains ANY 13-to-19-digit run except the key AND the rendered label of the single row criterion 6.1 declares as carrying a legitimate structured reference, which this test names explicitly as its one permitted exception, the key and the label being the SAME STRING for an unresolved group (src/modules/merchants/domain/ merchant-review.ts:86 and src/modules/overview/domain/ month-projection.ts:83 both set the label to the normalised text that is the key), so the exception must cover both or the criterion is satisfiable only by the accident of that row happening to be resolved in the scenario (finding PR5-002); and that the display helper renders such a run masked to its last four digits. npm run test:e2e exits 0 with the merchant review and month view specs asserting the same two sweeps over rendered group labels. The exact sweep and the named exception exist because the general sweep alone contradicted 6.1 (finding PR4-001): measured over the real statement, every structured reference is 13 or 15 digits and 5 non-card descriptors fall inside the 13-to-19 window, so a faithful fixture reference cannot avoid it and the only way to satisfy both criteria without this exception is a sub-13-digit reference, which would disarm the guard criterion 6.1 exists to provide.
- id: 6.4
  criterion: npm test exits 0 with a round-trip test: the value the review form submits for a card group is the UNMASKED normalised text, assigning a merchant from it writes an EXACT rule whose pattern equals that text, and re-running resolution over the fixture resolves every row of that merchant, including a row from the other month that was not the one named.
- id: 6.5
  criterion: git diff --name-only against the phase base contains neither src/modules/import/domain/dedup.ts nor src/modules/ledger/domain/corrections.ts, npm test exits 0 with test/domain/dedup.test.ts unmodified, and an import test re-importing the card fixture reports zero rows added and all rows known, so no stored dedup key moved. FACTS UNTOUCHED (finding PR3-006): the same test asserts that after this phase's changes the stored rawLine and rawContent of the card fixture's rows are byte-identical to what the pre-change pipeline stored and still carry the unmasked card-number run, so masking never reached the facts layer (prisma/schema/import.prisma:106, CLAUDE.md non-negotiable 5) and the M3-P2 re-parse contract still holds.
- id: 6.6
  criterion: The work history records the measured deployed MerchantRule count, the branch selected under the CR-402 contract and the command that produced the count; the pinned regression table in test/domain/normalise-counterparty.test.ts is updated in the same commit as the recipe change; and if the count was nonzero, npm test exits 0 with a migration test whose input patterns are produced by the PRE-change recipe, either pinned as literals captured before the recipe is edited or produced by running the previous implementation, NEVER by calling the new normaliser, and which asserts for each such pattern that its re-normalised value equals the NEW normalisation of the raw descriptor it was created from (finding PR3-005: the contract's idempotency, new(new(x)) equals new(x), makes a self-seeded test green for any recipe whatever; the property the migration needs is new(old(x)) equals new(raw), and a strip anchored to a token the old recipe already removed fails exactly there). RE-MEASURED AT THE HEAD (finding PR3-007): the MerchantRule count is measured again at the release head with the same pinned read-only command and recorded; if it is nonzero while the verification-first measurement was zero, the phase does not close until the re-normalisation branch ships.
- id: 6.7
  criterion: npm run test:e2e exits 0 with a phone-viewport spec at 390x844 asserting the month view and the merchant review render their group labels masked and document.documentElement.scrollWidth does not exceed 390 on both screens. The spec sets its own viewport in the spec file (Playwright test.use, the shape test/e2e/import.spec.ts:160 and test/e2e/navigation.spec.ts:178 already use), so no change to playwright.config.ts is needed and this phase's files-to-touch stays accurate (finding PR3-008); if M3-P4's mobile project has already merged, the spec may run under it instead, which is additive either way and is why conflicts-with stays empty.
- id: 6.8
  criterion: Privacy gate, both halves executed with a probe list read from the real PDFs in the container uploads at review time and never committed: (a) git grep across the whole worktree for the real statement's identifiers, holder name, card number in every grouping shape and each merchant string from it exits 1; (b) over the files this phase adds or modifies (git diff --name-only against the phase base), the same grep extended to all five promised categories (identifiers, names, merchant strings, thousands-form amounts and dates) exits 1. If the real PDFs are absent from the executing container, this criterion is recorded as not-witnessed in the work history and the review escalates for a container that has them, never marked green by vacuity.
- id: 6.9
  criterion: npm run typecheck && npm run lint && npm test exit 0 and npm run test:e2e exits 0 at the phase head, including the unmodified golden journey.
- id: 6.10
  criterion: grep for the merchant-source expression over src/, where the grep is grep -rlE 'counterpartyName \?\? [A-Za-z_]*\.?description', a literal operand-bearing pattern rather than a prose description of one (findings PR4-003 and PR5-001), matches exactly THREE files: the module exporting the shared helper, src/modules/import/domain/dedup.ts and src/modules/ledger/domain/corrections.ts, the last two being the frozen sibling recipes that keep their own copies by design; and grep for the shared helper's module path over those two files exits 1, so neither frozen recipe imports it. This is what witnesses decision D-11's operative content, the single definition, which no other criterion can see (finding PR3-004); the pin allows three files rather than one precisely because criterion 6.5 exists to keep the two frozen copies where they are. THE OPERAND IS PART OF THE PATTERN, not decoration (finding PR5-001, executed by the scout against the tree at this plan's head rather than asserted): the bare literal "counterpartyName ??" matches SIX files today, because src/modules/import/adapters/import-repository.ts:271 and :360 write a persistence mapper's null-coalescing and src/modules/import/ui/profile-confirmation.tsx:100 renders a preview cell, neither of them the merchant-source rule and neither in this phase's files-to-touch, so a bare-literal pin would match FIVE after unification and could be reached only by editing unrelated code. The operand-bearing pattern above matches exactly FOUR files today (interpret-window.ts, merchant-review.ts, dedup.ts, corrections.ts) and exactly THREE after the unification, which is the number this criterion demands. POSITIVE HALF (finding PR4-003): grep -l for the shared helper's import specifier over src/ matches BOTH former call sites, src/modules/ledger/application/interpret-window.ts and src/modules/merchants/domain/merchant-review.ts, so the unification is witnessed by what those call sites import and not only by what no longer appears; a review reading of the diff confirms no fourth definition was written in an equivalent form the literal pattern cannot see, such as a ternary on undefined.

### hazard-classes
- id: H6.1
  statement: The dedup normaliser or the ledger's refund key edited along with the merchants one, rewriting stored dedup keys and turning the next re-upload into a full duplicate import; or the reverse error, a unification sweep that pulls the two frozen copies into the shared helper.
  addressed-by: criterion 6.10
- id: H6.2
  statement: Over-stripping: a strip broad enough to collapse the card noise also collapses genuinely distinct merchants that share a chain prefix and a city into one group, which is silent and looks like success.
  addressed-by: criterion 6.2
- id: H6.3
  statement: The recipe change silently detaching stored MerchantRule patterns (finding CR-402), so the owner's namings quietly stop applying while totals stay right.
  addressed-by: criterion 6.6
- id: H6.4
  statement: Masking applied to the grouping key or to the value the review form submits, producing an EXACT rule that can never match.
  addressed-by: criterion 6.4
- id: H6.7
  statement: Masking applied to rawLine or rawContent, editing an immutable fact and destroying the M3-P2 re-parse contract, which is the plausible reading of "mask where the descriptor is rendered" for an implementer who thinks of the label as starting at the fact (finding PR3-006).
  addressed-by: criterion 6.5
- id: H6.5
  statement: Real statement content reaching the repository through the card fixtures written to reproduce the descriptor grammar.
  addressed-by: criterion 6.8
- id: H6.6
  statement: The full card number surviving into a rendered label or a screenshot after the phase, the exposure the owner reported.
  addressed-by: criterion 6.3

### migrations
No schema migration. One CONDITIONAL data migration, selected by the verification-first step's measured count: a one-off re-normalisation of stored MerchantRule EXACT and PREFIX patterns under the CR-402 contract, shipped in this change if the deployed database holds any rule rows, and recorded as not needed with the measured count if it holds none.

### conflicts-with
(none)

### parallelizable
false

### citations
- src/modules/ledger/application/interpret-window.ts:87
- src/modules/merchants/domain/merchant-review.ts:50
- src/modules/merchants/domain/normalise-counterparty.ts:16
- src/modules/merchants/application/assign-merchant.ts:54
- src/modules/import/domain/dedup.ts:18
- prisma/schema/import.prisma:106
- charter.yaml:41

### fill-in
filled: true
root-cause: Measured through the shipped pipeline: card rows carry no counterparty account, so the merchant key is the whole descriptor, which embeds the transaction's own date, amount, card number and holder; the shipped normaliser matches none of that grammar, so 15 card rows yield 15 keys and every key carries the card number the labels render.
fix-shape: One merchant-source helper used by both call sites, card-grammar strip patterns in the merchants normaliser with the pinned table updated and the CR-402 branch discharged, a display-only masking helper on both label surfaces, and synthetic fixtures witnessing collapse, non-collapse, masking and the unmasked rule subject.
files:
  - src/modules/merchants/domain/normalise-counterparty.ts
  - src/modules/ledger/application/interpret-window.ts
  - src/platform/ui/
  - test/fixtures/

# Environment warnings

# Fleet environment warnings

Appended to every composed brief. Facts, measured in this fleet's container.

1. AMBIENT DATABASE CREDENTIALS BELONG TO ANOTHER PROJECT. The shell
   environment carries DATABASE_URL, DIRECT_URL and SUPABASE_* variables
   pointing at a deployed Supabase pooler for a different project (Hemma).
   Shell env OVERRIDES .env for Prisma. Never run a database command with
   ambient env: pin connection strings explicitly to the local stack (or
   the pulse deployed project when, and only when, the task is
   deploy-verify). The first unpinned migrate attempt in M1-P1 targeted
   the foreign host and failed with P1001; nothing executed, but only
   because the host was unreachable.
2. Node: the container default is Node 22; the tiphys CLI needs Node 26.
   For any tiphys command run first: export NVM_DIR=/opt/nvm &&
   . /opt/nvm/nvm.sh
3. Prisma 6.19 has an AI-agent consent guard on migrate reset; npm 11.19
   blocks lifecycle scripts unless package.json allowScripts covers them
   (committed in the repo's package.json). Both are handled in the
   repository; do not fight them ad hoc.
4. Outbound HTTPS goes through a TLS-reterminating proxy with CA bundle
   /root/.ccr/ca-bundle.crt. npm is pre-wired. Do not disable TLS
   verification anywhere.
5. Deployed database endpoints for the pulse Supabase project: the
   direct connection AND the transaction pooler both live on the
   IPv6-only db.ygsarzjqosqmkqibqogk.supabase.co host and are
   unreachable from IPv4-only environments (Vercel included). The
   SESSION POOLER (aws-0-eu-central-1.pooler.supabase.com:5432, project
   ref in the username) is the only IPv4 endpoint and serves BOTH
   DATABASE_URL and DIRECT_URL in deployed environments. Local work
   keeps using the local supabase stack, pinned per warning 1.
6. Ambient foreign SUPABASE_* and DATABASE_URL variables override .env for
   the Playwright webServer as well as Prisma: when running npm run
   test:e2e, pin all five values (DATABASE_URL, DIRECT_URL,
   NEXT_PUBLIC_SUPABASE_URL, the publishable key and the secret key) to
   the local stack in the invoking shell, not only the Prisma pair.
7. The local auth stack accumulates e2e users across runs: any supabase
   admin listUsers existence check must paginate (the one-page check in
   prisma/seed.ts broke db:reset past 50 auth users; fixed in M1-P4).
8. Playwright positional test filters substring-match the full path
   INCLUDING the worktree directory name; in fleet worktrees use -g to
   target tests. After sudo dockerd, the supabase auth container needs
   roughly 30 seconds before seeding or admin API calls succeed. Next.js
   infers a workspace root when multiple lockfiles are visible; fleet
   worktrees can trip this, pin turbopack.root or ignore the warning
   knowingly.
9. The real statement uploads' FILE NAMES themselves embed identifiers
   (an IBAN plus statement date; a document reference). Never write the
   full filenames into any note, commit, or report: refer to the uploads
   by their 8-hex prefix only (0f79fa3d = KBC card, 39bada64 = Belfius
   current account; the M3-P2 dispatch had this mapping backwards). One
   leak via notes happened in M3-P2 and was scrubbed with a history
   rewrite before anything consumed the branch.
10. The container's disk fills up: each worktree clone carries ~1GB of
    node_modules, and a full root filesystem manifests as MOVING
    chromium "Page crashed" e2e failures (CR-903 root cause, 54MB free
    at the time), not as a disk error. Before dispatching gate or e2e
    work, check df; delete node_modules/.next from closed-phase
    worktrees freely (regenerable; commits live in git).
