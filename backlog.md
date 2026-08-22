# Backlog

- Kernel 0.1.0 spawn scrubs the child environment (M2-P8 allowlist) and ships no agent adapter (PR-013 deferred), so a real agent payload cannot authenticate under tiphys spawn. The M0-P1 plan-writer was dispatched as a harness agent with the composed brief, incremental output, and a freshness watchdog armed at dispatch. Pilot tuition for the kernel: an agent adapter or a sanctioned per-invocation allowlist extension is needed before spawn can carry real work.

- v0.2 intake landed: delivery/intake/pulse-v0.2-pdf-addendum.md in the pulse repo (PDF import, layout templates, balance contract, mobile share-sheet upload). Plan iteration v0.2 starts from it after v0.1 ships; PR-0xx style planning waits for the DR-0002 checkpoint unless the owner pulls it earlier.

- Kernel 0.1.0 verdict validation in a downstream fleet: the mandated --context . form cannot pass as documented (plan.yaml, work-history.yaml, assurance-modes.yaml are resolved at the context root, where a project keeps them under delivery/ and the fleet root), and dual-review-decorrelation hard-requires two produced-by families, which DR-0003 overrides for this pilot. Both M1-P1 reviewers escalated instead of recording a false family; merge-gate arbitration cites DR-0003. Kernel tuition: the check needs a declared same-family override path, and context resolution needs project-layout awareness.

- Fleet-container tuition: Playwright chromium cannot reach vercel.app through the egress relay (TLS dies in-tunnel; curl unaffected; CA, ECH, ML-KEM ruled out). Production-URL e2e runs need either a relay fix or an owner-side runner. Recorded during M1-P1 deploy-verify.

- M1-P1 CLOSED including deploy-verify (2026-08-18). Next dispatch: M1-P2 (import), which also carries the RLS migration and the two tracked lows CR-006/CR-007 from the M1-P1 review.

## M1-P2 merged-with low findings (DR-0012 condition 2 tracking)

- CR-208 (hazard delta, low): a negative value inside a Debit-headed
  column parses sign-inverted; pre-exists on the two-populated-column
  path, F3 only widened reachability. Fix: fail loud on signed values in
  directional columns. Owed to the next import-touching round (M1-P3 or
  first parser follow-up).
- CR-209 (hazard delta, low): parseSourceProfileSpec accepts
  case-colliding indicator tokens (debitValue "X", creditValue "x"),
  making the credit token unreachable. Fix: reject equal-after-uppercase
  pairs. Same owner as CR-208.
- M1P2-C6: ANSWERED by the owner (2026-08-19), question judged empty.
  Owner's model: transactions are on an account; cards are access to
  perform transactions on an account; this version does not care who
  performed a transaction, only what the transactions are. For a credit
  card the statement is the account statement and there is only ever a
  one-one card-account relationship in this household, so the
  two-same-format-cards scenario does not arise. Binding behaviour stays
  as shipped (spec-equality plus the landing-account transparency from
  fix round 1). No code change owed.

## Tuition: M1-P2 merge landed the wrong head (2026-08-19)

PR #9 was squash-merged while origin's claude/m1-p2-import still pointed
at 10beb40: the implementer's report claimed the fix round was pushed
(10beb40..41ff037) but the push never landed, and the orchestrator
merged without comparing the PR's head sha to the reviewed head. Main
carried the pre-fix tree for about ten hours; the deployed build did
too. Caught by the M1-P3 implementer's base verification (CR-209's fix
target missing from main). Repair: reviewed head pushed, main merged
into the branch with the branch side kept for double-changed files
(main's side was by construction the branch's own older version), gates
re-run on the reconciled tree, squash-merged as PR #10 (main 6fc43c9)
with the head sha verified against the PR object this time. Standing
rule from this tuition: a merge is dispatched only after reading the PR
head sha from the PR object and matching it to the reviewed head, and a
reported push is verified with git ls-remote before any paperwork cites
it.

## M1-P3 merged-with low finding (DR-0012 condition 2 tracking)

- CR-308 (hazard delta round 2, low): stripCurrencyNoise is a single
  regex pass, not a fixpoint, so an adversarial interleaved-letter cell
  (EEURUR-742,10) reconstructs a signed value after one strip and slips
  the unsigned entry point; every realistic export shape is refused.
  One-line fix (reject negative magnitude in the unsigned entry, or
  strip to fixpoint) plus a red witness. Owed to M1-P4 or the next
  parser-touching round. The fix commentary's absolute claim ("can
  never diverge again") is falsified by this construction (R-087);
  correct the comment with the fix.

## Hygiene, tracked

- Pre-existing em dash in messages/*.json merchantsBody (CLAUDE.md rule 7
  violation, on main since before M1-P4, found by the M1-P4 implementer,
  outside its scope). Owed to M1-P5, whose declared scope includes the
  catalogs.

## M1-P4 merged-with low finding (DR-0012 condition 2 tracking)

- CR-404 (hazard delta, low, R-087): comments at the one-primary
  migration and the MerchantTag model claim the partial index is
  "asserted by name and predicate", but the suite's regex accepts any
  index name (table, column and predicate ARE pinned). One line: pin
  the name in resolve-merchants.test.ts's regex or reword both
  comments. Owed to M1-P5.

## DR-0002 feedback log (owner, first live use, 2026-08-21)

1. NAVIGATION MISSING, phase-blocking for use: the app has no general
   navigation. The month view's empty state names the import action with
   no link, and once data exists there is still no way to reach /import
   or /merchants from the header. Owner's words: "no general navigation
   so later imports past empty screen also won't work."
2. PDF IMPORT IS ESSENTIAL, testing halted: "lack of pdf import makes it
   close to unusable. pdf import path is essential for me. I won't test
   anymore until it exists." The owner exports PDFs on mobile; CSV
   requires PC banking. Sequencing consequence, owner-directed: the v0.2
   PDF import addendum work comes BEFORE the DR-0002 one-real-month
   test; DR-0002 stays open, its clock starts only when the owner can
   actually feed Pulse from their phone.

Disposition: v0.2 planning cycle dispatched (dispatch-plan-v02.yaml,
scout phase M0-P2). The navigation defect rides into the v0.2 plan as
an early phase per the owner feedback. Real statement PDFs exist in
this container's uploads directory as format reference for the planning
and build work; they are NEVER committed, fixtures stay synthetic.

## Tuition: desktop-blind UI criteria (2026-08-21)

The M3-P1 navigation shipped desktop-focused and broke on the owner's
phone (sign out pushed off screen) despite zero-finding reviews: every
gate and both reviews ran Playwright's default desktop viewport because
the phase criteria never named a phone viewport, and the plan had
parked mobile-first work at M3-P4. The reviews verified a blind
contract faithfully; the contract was wrong for a mobile-primary owner.
Standing rule from this tuition: every UI-touching phase's acceptance
criteria MUST include a phone-viewport (390x844) assertion covering the
phase's surfaces (visibility of all controls, no horizontal scroll),
and UI review dispatches name the owner's device class explicitly.
Applied: pulse main 0a49ef7 adds criterion 1.5 to M3-P1; a defect round
is running; M3-P2 and later phases inherit the rule at dispatch.

## M3-P2 merged-with lows (DR-0012 condition 2 tracking)

- HZ-001-R (hazard delta, low): a corrupted margin first-transaction
  line with a zero amount drops silently (continuity has no lower
  anchor, zero preserves the sum). Documented in code; not adversarially
  reachable. Concrete fix on file (anchor continuity to the expected
  first sequence). Owed to the next parser-touching round.
- HZ-003-N (hazard delta, low, cosmetic): a fingerprint-matched
  structure failure labels "unparseable" rather than
  "layout-unsupported"; zero rows either way. Same owner.
- HZ-004 (standing, plan-parked): cross-format duplication for the same
  account-month (CSV plus PDF). Escalated to the owner 2026-08-21; the
  owner is PDF-only in practice; stays parked unless the owner plans to
  mix formats. Tripwire plan edit remains the recommended shape if it
  ever activates.

- HZ-007 (deploy-defect round, low): extractPages collapses both
  extractor error kinds before the port, leaving dead mapping branches;
  diagnosis channels sit below the collapse so no behavioural defect.
  Concrete edit on file (propagate the extractor's Result). Owed to the
  next parser-touching round alongside HZ-001-R and HZ-003-N.

## Owner test of the deployed Belfius PDF import (2026-08-21)

The parse itself is correct: dates, signs, integer cents, counterparty
IBAN on transfer rows, the settlement debit, and multi-line description
joining all match the statement. Two real defects and one decision came
out of the owner's screenshot, measured against the real statement:

1. MERCHANT GROUPING IS DEAD FOR CARD ROWS (high). merchantText falls
   back to the full description when counterpartyName is null
   (interpret-window.ts:87-88), which is every card payment. The
   descriptor embeds the transaction's OWN date and amount plus the card
   number and holder name, so every row is a unique string: measured on
   the real statement, 15 card-payment rows normalise to 15 distinct
   strings where there are only 5 real merchants. Consequence: naming a
   merchant writes an EXACT rule that can never match a second
   transaction, and the month view degenerates to one group per row.
   The M1-P4 normaliser strips terminal noise, masked PANs, city and
   date fragments, but none of its patterns match this real shape (the
   embedded "18/06", "3,55 EUR", "KAART NR <16 digits>", "- Name", and
   Belfius's own literal "<B>" marker all survive uppercase-only).
   Root class: normalisation was built against assumed shapes, never
   verified against a real descriptor. Same family as the mobile
   viewport gap: no criterion demanded it.
2. FULL CARD NUMBER RENDERED (medium, owner decision). Belfius prints
   the unmasked PAN in the statement text (verified: two distinct
   grouped-16 sequences in the real file), so storing it verbatim in
   rawLine is correct and immutable by design. Rendering it in the
   descriptor column is avoidable exposure: it appeared in the owner's
   screenshot. Recommend masking at the DISPLAY layer only, facts
   untouched.
3. Descriptor readability on mobile follows from 1: once the normalised
   merchant exists, the raw text should stop dominating the row.

Disposition: proposed as phase M3-P6 (real-descriptor merchant
normalisation plus PAN display masking), with the real statement's
descriptor shapes as the fixture source, synthetic only.

## Owner question, 2026-08-21: the five-row preview

Answered from the code, not from memory: the confirm screen's preview is
`rows.slice(0, 5)` (import/[id]/page.tsx:76), while confirming ingests
`parsed.value.rows`, the FULL parsed set (confirm-import.ts:115-126).
So all rows import; five is a deliberate format-confirmation sample.
Verified on the owner's re-uploaded statement (prefix 84742d93, same
statement as 39bada64): 5 pages, 39 transaction rows detected.

REAL GAP the question exposes: the confirm screen never states how many
rows were found. The owner is asked to approve a format from five rows
with no way to check the parser found 39 rather than 4, and the copy
"First five rows, as Pulse reads them" implies a total that is never
shown. The balance gate catches most row loss but the hazard review
already found its zero-sum blind spot, so the count is the owner's own
cross-check against the paper statement. Proposed: show the parsed row
count (and, once M3-P3 lands, the balance identity's own numbers) on
the confirm screen before the decision. Natural home is M3-P4 (mobile
import flow, already touches this UI); needs a plan phase amendment
since M3-P4's criteria do not name it.

## Owner verdict on mobile, 2026-08-21: 2/10 quality, 1/10 usability

Owner, on the deployed month view at phone width: "looks bad to say it
politely... I can't use this in any form. the total lack of mobile
first in this whole project."

Measured root cause, not a guess: `grep -c "@media" src/app/globals.css`
returns 0. There is no phone layout anywhere in the product. The month
view is `.month-grid { grid-template-columns: minmax(0,1fr)
var(--layout-rail) }` at globals.css:681-683, an unconditional desktop
two-column grid with a 372px fixed rail. Every screen was built to the
committed desktop prototype; the only mobile accommodation in the tree
is the flex-wrap added reactively in M3-P1's defect round.

ORCHESTRATOR ERROR, named plainly: after the owner's first mobile
report I set the standing rule as "phone viewport 390x844: controls
visible and clickable, no horizontal scrolling". That is a bar this
screen PASSES while being unusable. I encoded the symptom the owner
reported instead of the requirement the owner has, which is a product
usable on the device they actually use. Every phase since has inherited
that too-weak bar and passed it.

In flight and relevant: M3-P6 (in delta review) replaces descriptor
blobs with merchant names, masks the card number, and fixes the 424
overflow on this exact screen. That materially improves it and does not
make it good: it stays a desktop table on a phone.

Proposed: a mobile-first phase for the month view and merchant review
with criteria that measure usability rather than overflow, plus
promoting mobile-first from a per-phase assertion to a declared
project constraint (the pulse-frontend skill currently says
desktop-first at SKILL.md:113; M3-P4 was already scoped to revise that
one line, which is far too narrow a fix).

## M3-P6 merged-with lows (DR-0012 condition 2 tracking)

- CR-05 (orchestrator-held): npm ci fails under npm 10 / Node 22 with a
  missing @swc/helpers lock entry while package.json pins engines 22.x.
  Not introduced by M3-P6; probed not to threaten the deployed build (no
  workflow runs npm ci). Base-branch chore.
- M3P6-C20 (from HZ-12): the bare Dutch card-number label word remains
  reachable by an ordinary Dutch noun standing before four four-character
  groups. The implementer declined the field-boundary anchor with a
  measurement (the preceding token is two letters on 15 of 23 real card
  rows, so a short-token anchor still admits the French idiom the finding
  named) and narrowed the vocabulary instead: bare form Dutch-only, with
  negative pins. Unreachable on all 215 observed descriptors. Revisit
  when a second bank grammar lands.
- M3P6-C21 (from CR-11): nothing executes a rendered gap-row STRING
  assertion; the site is covered by the derivation test, and the e2e now
  asserts the zero so the claim cannot go stale. A fixture producing gap
  rows would close it.
- Standing from the hazard lens: a card number printed with NO label is
  masked by nothing (the declared price of grammar anchoring), and prose
  in files a branch does not touch has no executable check.

## M0-P3 corrections to the orchestrator's dispatch (2026-08-22)

ORCHESTRATOR ERROR, corrected by the M0-P3 plan-writer and verified
independently. dispatch-plan-mobile.yaml's verified-root-cause states that
`.month-grid` is `grid-template-columns: minmax(0, 1fr) var(--layout-rail)`,
a fixed two-column desktop grid. That was the text at e4ea3ba. At b560e85 it
is a wrapping flex (globals.css:704 to :719), changed by 5f7b62c, so the
top-level grid already collapses on a phone. I wrote a root cause from a
stale reading and did not re-check it against the head I was dispatching on.

What survives is still enough to carry the phase: `grep -c "@media"
src/app/globals.css` returns 0, and globals.css:564 says "Desktop first". The
real defect is one level down, at globals.css:756 (.month-columns) and :781
(.month-row): three-column grids whose two amount tracks each carry
`min-width: var(--space-14)`, 80px (tokens.css:121). Two 80px tracks plus two
gaps inside 390px is what leaves the name column nothing.

SETTLED, and recorded so M3-P7 does not spend a round on it. The plan-writer
could not verify whether a viewport meta tag exists, could not render a page,
and correctly recorded it verified: false as the amendment's highest-impact
unverified claim. It does exist. Next's App Router injects it by default:
`<meta name="viewport" content="width=device-width, initial-scale=1"/>`,
observed in the live deployment's rendered head at /sign-in on 2026-08-22 and
in a page capture taken earlier in the session. The phone is receiving
device-width. The unusability is layout alone, not a missing viewport tag.
Criterion 7.2 is still worth keeping as a pin against regression.
