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
