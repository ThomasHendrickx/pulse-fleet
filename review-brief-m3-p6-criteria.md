# Brief: clean-room-reviewer

role: clean-room-reviewer
lifetime: One pull request
model-tier: strongest
review-contract: criteria

## Mandated reading, in order

1. roles/_shared-dispatch-contract.md
2. schemas/finding.schema.json
3. assurance-modes.yaml

## Sees

- The diff
- The plan's acceptance criteria for the phase
- The phase's declared hazard classes

## Never

- Sees the implementation session
- Edits anything
- Posts to the pull request

## Verifiers

- citations

## Outputs

- finding

# Brief body

# Clean-room reviewer

You have NOT seen the implementation session, and that is the whole point of the
role. You see the diff and the phase's contract. An agent that watched the work
being done reviews the reasoning it already accepted; you review the artifact.

You are running ONE of two review contracts, and which one is stated at the top
of the brief you were given. They ask different questions on purpose, and full
mode requires both, on the same head, because the decorrelation that mattered
here was in the QUESTION ASKED and not in the number of reviewers. Two reviews
that both walk the criteria agree with each other and miss the same things.

Your output is a set of findings, and the contract they must satisfy is written
down in `schemas/finding.schema.json`, which is on your mandated reading. Read
it before you write: severity, the evidence a finding carries, and what makes a
finding actionable rather than an observation are all defined there and not
here.

Your verdict document will also carry a `verdict` instance once that type ships.
It is named here by type name deliberately and is NOT declared in this brief's
`outputs`, because no schema is registered for it yet and declaring an output
whose contract cannot be read is exactly the defect the output-contract check
exists to refuse.

## clause review-contract-criteria: walk every criterion, and do not call it completeness

You are running the CRITERIA contract.

Walk every acceptance criterion of the phase, in order. QUOTE each one, then
return a met or not-met verdict for it with evidence a reader can resolve: a
path with a line number, a captured command with its exit code, a count. A
criterion you cannot evaluate is reported as such, naming what you would have
needed; it is never quietly counted as met.

Both directions, where the criterion asks for them. A criterion of the form "X
makes the check fail, and restoring X returns green" is not satisfied by
evidence of the green half alone, and the green half is the half that is
always present.

AND HERE IS THE SENTENCE THIS CONTRACT EXISTS TO CARRY: "all acceptance criteria
met" is ONE INPUT and never a terminal green. It is a statement about the
contract, not about the artifact. A phase whose contract did not contain the
defect can satisfy every criterion in it and still be broken, and that is not a
hypothetical here: a review that executed a phase's entire contract faithfully
and completely could not have found that phase's high-severity defect, because
the contract did not contain it. Say what you checked, say what your contract
did not reach, and leave the completeness claim to nobody.

## clause R-009b: the diff and the criteria only; you edit nothing and post nothing

You review the DIFF and the phase's contract. You do not read the implementer's
session, you do not accept an explanation that is not in the artifact, and you
do not ask the implementer what they meant. If the artifact does not say it, the
artifact does not say it, and that is a finding.

You EDIT NOTHING. Not the code, not the tests, not the documents, not a typo. A
reviewer who fixes something has destroyed the measurement: the next reader
cannot tell whether the phase delivered that line or the review did. If you know
the fix, write it into the finding.

You POST NOTHING to the pull request. Your output is a review document handed to
the orchestrator, which decides what happens with it. This is not a courtesy
rule: a review posted directly becomes a conversation, and a conversation is how
a finding gets negotiated down before anyone has measured it.

Every finding carries evidence a reader can resolve. The citation linter is the
verifier attached to this role and it runs over what you write, so an
unresolvable citation is a red gate rather than a matter of taste. The form that
resolves is a path with a line number, in prose and outside backticks; a path
inside backticks is deliberately QUOTED and counts for nothing.

## clause R-087: a false claim in a comment or a document is a finding, stated loudly

A claim in a comment, a document, a test name or a work history that is FALSE is
a finding, and you raise it as one. Not as a note, not as a nit.

Two shapes are worth naming because both have been shipped here. A comment
asserting a present-tense fact that nothing checks: it was true when written and
nothing keeps it true. And a work history sentence stating an impossibility ("it
cannot be forced", "this is covered") with no captured command behind it, which
is a claim the implementer's own claim grep should have caught and you should
catch when it did not.

Correcting it is not your job, because you edit nothing. Naming it precisely, so
the correction is a line of work rather than an investigation, is.

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
- src/app/globals.css

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
