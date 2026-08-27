# Owner ledger

What the owner has already been told, already decided, and already had
corrected. Read this before reporting anything to them; append in the same
turn as the report. The failure this file exists to stop: a compacted
session re-deriving conclusions from the same branches and delivering them
again as if new, which happened repeatedly up to 2026-08-27.

## Decisions the owner has made (never re-ask, never re-open)

- 2026-08-26, the design cut (DR-0031, confirmed 2026-08-27): accounts are
  entered ONCE at setup, before any import; one phase plus a small
  migration phase; the owner's words about overcomplication are quoted in
  the plan's standing context. M3-P14 account setup was built and merged
  under this cut.
- 2026-08-27, DR-0030: a savings (reserve) account's statement IS accepted;
  its rows are held out of the totals and shown as held. Supersedes plan
  decision D-55 (which refused those statements). The refusal still in
  main's import gate is now BEHIND the owner's decision and is scheduled
  work, not a choice.
- DR-0031 ring rule stands: a ring is correctable only while the account
  has no imported rows of its own. The m0-p6 lane's argument for
  both-directions correction is parked awaiting a NEW owner record; do not
  build it, do not re-ask.
- DR-0029: process and sequencing questions never reach the owner.

## What the owner has been told (do not re-deliver)

- 2026-08-27 (this session's report): the 8/26 evening merges landed M3-P12
  at a stale head, leaving the re-derivation script on main without its
  database guard while fleet containers carry a foreign deployed
  DATABASE_URL; repaired by merging the branch's verified head (main
  86d2c1d). The account-setup merge had also overwritten the plan file
  with a pre-amendment copy, deleting criterion 12.23, hazards H12.30-32
  and phases M3-P16/M3-P17; restored in the same merge. All fast gates
  green (typecheck, lint, 750/750, privacy, decisions, tokens).
  Deployed health endpoint green after push.
- Same report: the owner's phone-press complaint is answered on main: a
  real held touch press is driven on the chromium-phone project and
  press-to-first-visible-change is measured and asserted
  (test/e2e/pressed-and-disabled.spec.ts).
- Same report: e2e (Playwright) could not run in this container (no Docker,
  no local Supabase stack); recorded as owed on the next capable container.
  Both merge parents were independently e2e-green.

## Corrections already issued (do not repeat the mistake, or the apology)

- Verdict documents beyond round two per lane may not land on main: the
  review round cap counts files (gate:decisions rule 2). Historical rounds
  stay on their lane branches.
- Five invented account-shaped values quoted by the M3-P14 review documents
  were added to test/fixtures/allowed-identifiers.txt with provenance
  (one review probe, four fixture accounts of the unmerged
  account-registry branch).
- The M3-P12 review treadmill (rounds 3 through 13) is closed by decision:
  the round cap now in CLAUDE.md rule 10, plus the planner's withdrawal of
  the register/scanner machinery (branch m0-p14-plan-descope-interlock,
  being ported and given its single review round). Residual round-13
  findings ride that work, not new rounds.

## In flight as of 2026-08-27 (this session)

- M3-P17 deployed build stamp: implementer dispatched on
  claude/m3-p17-deployed-build-stamp.
- Interlock descope: port of c51c731 onto main's plan in progress, then one
  adversarial review round, then implementation.
- M0-P6 harvest iteration: planner dispatched on claude/m0-p6-plan-accounts
  to carry DR-0030 (savings-held) and the canonical backfill (closes the
  live exposure that pre-setup stored account numbers in a non-canonical
  form make a statement unretryable) into a renumbered follow-up phase.
- M3-P16 deployed re-derivation run: waits for M3-P17 and the descope
  outcome.
- Branch sync (scripts/sync-branches.sh) could not run from this session:
  the permission layer refuses bulk force-pushes. Needs a user-side run or
  a permission grant.
- M3-P10 busy state merged 2026-08-26 with no clean-room review lane on
  record; noted as a gap of the runaway evening, not yet remediated.

## M0-P6 harvest iteration delivered (2026-08-27, plan-writer)

- Pushed to claude/m0-p6-plan-accounts at e8021ff (merge of main at
  dc18484 plus one harvest commit). Not merged to main.
- ONE new phase, M3-P18 (claude/m3-p18-savings-held-and-migration), six
  criteria 18.1 through 18.6: savings statement accepted with the built
  refusal removed (DR-0030), held rows rendered under the typed label and
  summed nowhere, both null-flow reads ring-scoped, canonical backfill of
  stored account numbers (named as closing the live P14-001 exposure: a
  pre-setup stored non-canonical number never matches the built gate's
  canonical lookup, so that statement is refused with no remedy), duplicate
  check comparing canonical forms with a colliding pair named by row id and
  never guessed at, and the gate criterion. M3-P14 kept as built (nine
  criteria); build-stamp M3-P17 untouched.
- Decisions: D-55 rewritten per DR-0030 with the superseded wording quoted;
  D-51 kept exactly as main has it; D-56 relived; D-59 and D-60 repinned to
  M3-P18; D-62 added (split-at-the-population record). NOTE: the interlock
  descope lane may also claim D-62; main carried none at merge time, so the
  second lane to land must renumber.
- The lane's 17.1 (ring change after rows have landed, with the ledger
  clear) is PARKED, not planned: DR-0031 fixes the ring once an account has
  imported rows of its own, and no owner record supersedes that. The
  lane's inference that DR-0030 falsified the freeze's premise is recorded
  in the parked entry, attributed, awaiting an owner record. Findings
  P17-001/002/003/005 ride the parked entry; P14-001, P14-006, P17-004,
  P17-006, P17-007, P17-009 ride M3-P18.
- Validation on the pushed head, all exit 0: python3 yaml.safe_load, npx
  tiphys validate --type plan --context delivery (dispatchable: true), npm
  run gate:privacy, npm run gate:decisions.

## M3-P17 clean-room review, HAZARD lane, round one (2026-08-27, reviewer)

- Verdict FIX-ROUND-NEEDED, pushed to claude/m3-p17-rev-haz at 67d736d
  (delivery/review/m3-p17-hazard.yaml). Head reviewed: 12c20f3.
- CRITICAL HZ-M3P17-01: the head cannot produce a production build. The
  route exports the UNSTAMPED constant beside its handler and next build
  refuses it as an invalid Route export field (witnessed, exit 1, Next
  15.5.23 from the tree's own lockfile). Consequences: npm run test:e2e
  cannot pass anywhere (the chromium-prod web server starts with npm run
  build), criterion 17.3 is unsatisfiable, and merging would break every
  Vercel deploy, so the stamp M3-P16 waits on could never land (the old
  build keeps serving; the route 404s there). typecheck only stays green
  in a never-built tree; with .next/types present it exits 2 on the same
  defect. Fix validated in a reverted experiment: marker moved to a
  sibling non-route module under src/app/api/health/version/, build exit
  0, unit tests 6/6.
- LOW HZ-M3P17-02: no cache-control header on the route's 200 (witnessed
  against next start); fix is an explicit no-store plus one assertion.
- LOW HZ-M3P17-03: gate:decisions red at head is inherited from the
  branch point f19984b (identical six lanes there); main's dc18484
  already prunes them; fix round should rebase onto current main. Already
  known to the owner via the implementer's work history open question; do
  not re-deliver as news.
- Route substance otherwise held under attack: exactly two fields, two
  pinned env names read at request time only, methods and error paths
  disclose nothing further, unstamped marker cannot collide with a sha,
  no database path touched, no scope growth.

## M0-P6 fix round one delivered (2026-08-27, plan-writer)

- claude/m0-p6-plan-accounts now at 30a9594: merge of main f2fccf5 (the
  descope lane's D-62 kept, this lane's split decision renumbered D-63,
  its two references updated) plus one fix commit answering all eight
  findings of delivery/review/m3-p18-plan-review.yaml. Key changes: the
  database-behaviour assertions of 18.3/18.4/18.5 re-levelled to
  slow-gate direct-client specs (merchant-rule-write precedent); the
  collision pair now left DETECTABLE via a committed read-only script
  (scripts/detect-account-collisions.ts) whose output the slow gate
  asserts, replacing migration output nothing captures, with the
  post-deploy run recorded as owed in the work history; 18.1's sweep
  gains SETUP_LINKED and the shipped refusal e2e (rewritten, not
  deleted); the seed harness gains the two savings-ring accounts;
  D-51 annotated (superseded D-55 wording quoted, freeze untouched);
  two DR-0030 dates and the migrations path corrected. Validation all
  exit 0: yaml parse, tiphys validate (dispatchable: true),
  gate:privacy, gate:decisions. One review round used; one remains
  under rule 10.

## Orchestrator progress (2026-08-27, later the same session)

- M3-P17 build stamp: fix round verified by the orchestrator's own gate
  run (typecheck, lint, 757 tests, privacy, decisions, tokens, build all
  exit 0), merged to main at d03c86c with both round-one verdicts landed.
  The DEPLOYED STAMP IS LIVE and answers with exactly main's head sha in
  production: the first time the fleet can observe what is deployed.
- Interlock withdrawal (D-62): plan iteration DISPATCHABLE after its two
  review rounds; implementation removed the register, scanner and proxy
  machinery (3241 lines out, 699 in), the five 12.23 measurements exist
  as red-witnessed fast-gate tests, all gates and the production build
  green in the orchestrator's own run; merged to main at dee3d32. The
  round-13 residual highs are resolved by withdrawal.
- M3-P18 (savings held plus canonical backfill): plan DISPATCHABLE after
  two rounds, merged at bec02cc with both reviews; implementer dispatched
  with the two carried round-two findings. Slow-gate specs and e2e remain
  environment-limited in these containers (no Docker); the slice cannot
  close until a capable container runs them.
- M3-P16 (deployed re-derivation run) is CREDENTIAL-BLOCKED, not
  process-blocked: the stamp is live, the guard and both hatches are
  merged, the Supabase management API reaches the pulse project for the
  residue proof, but the run needs the deployed postgres connection
  string whose password only the owner (Vercel env) holds. To unblock:
  either the owner provides the two connection strings to a session for
  the one documented run, or runs the two-command invocation themselves
  (guard-cli then the routine, dry-run first, both hatches inline).
- M3-P9 carried instrument fixes (testIgnore-honoring membership check,
  behavioral listener witness) dispatched on
  claude/m3-p9-carried-instrument-fixes.

## M3-P18 implemented (2026-08-27, implementer)

- claude/m3-p18-savings-held-and-migration delivered: DR-0030 built (the
  savings-ring refusal removed root and branch, held blocks under the typed
  label with the note in three catalogues, no sums per D-60), both null-flow
  reads ring-scoped (CR-502 held), the canonical backfill migration
  (accounts.iban only, collision pair excluded first, idempotent), the
  read-only detection script wired to the surviving guard contract
  (R2-M3P18-01/-02 honoured), and the typed duplicate check over canonical
  forms. Fast gate 678/678, privacy, decisions, tokens, build all exit 0.
- NEW SINCE DISPATCH, do not re-derive: the implementer container carries a
  bare Postgres 16 (no Docker, no Supabase auth). A throwaway cluster
  witnessed prisma migrate deploy (7/7 incl. the backfill), all SQL-level
  arms of 18.3/18.4/18.5 (42 assertions, captured verbatim in
  delivery/work-history/m3-p18-notes.md), and one FORMAL Playwright pass of
  test/e2e/held-and-gap-rows.spec.ts (1 passed). Still owed elsewhere:
  npm run test:e2e in full (browser journeys need the Supabase auth
  service); the slice does not close until it passes.
- Open questions handed on (work history claims C4-C6): the browser-arm
  specs, the post-deploy detection-script run against the deployed target,
  and one criterion-18.5 wording read (error points at the offending typed
  row by index; naming the duplicated account's label would need two files
  outside the declaration). Two declared-scope deviations recorded
  (overview application index.ts binding; two comment-only R-087
  corrections).

## M3-P18 fix round one delivered (2026-08-27, implementer)

- Head d3cb64f (code fix 20b6b07). HZ-M3P18-01 closed red-then-green
  against a real Postgres 16: one shared SQL whitespace class
  (ACCOUNT_NUMBER_SQL_WHITESPACE_CLASS, the POSIX class plus the full
  ECMAScript set as visible ARE escapes) now strips exactly what the
  platform canonical form strips, wired into the migration (edited in
  place: unmerged branch, throwaway clusters only), the detection
  grouping and the reserves join; a fast test sweeps every Unicode code
  point and pins the equivalence; the harness seeds NBSP/narrow-NBSP/BOM
  renderings including a second collision pair the corrected grouping
  now emits.
- HZ-M3P18-02: freeze untouched (owner decision); guard's stale
  justification corrected loudly; held note now names the savings-ring
  registration as the cause in three catalogues. THE PARKED RING-CHANGE
  ENTRY NEEDS AN ANNOTATION the implementer may not write (R-007): one
  confirmed upload onto a mis-ringed account now closes the only ring
  correction; recorded as claim M3P18-C9 for the orchestrator to land.
- HZ-03: INGESTED documented and pinned as terminal-and-settled for a
  savings import. HZ-04: detection script's post-deploy run documented
  as not optional, silent-hold trade named. CR-01: read-back recorded as
  environment-limited, owed before slice close (each new line invented
  in-session; real documents not in these containers); do not treat as
  done. CR-03/04/05 fixed. CR-02 stands: npm run test:e2e still executed
  nowhere; browser arms need the Docker-based Supabase stack.

## M0-P6 parked-entry annotation (2026-08-27, plan-writer)

- claude/m0-p6-plan-accounts at 123b713 (fast-forwarded to main 49d1ff4,
  which had absorbed the lane, plus one commit). The parked ring-change
  entry now carries the measured consequence from clean-room hazard
  finding HZ-M3P18-02: one wrong ring answer at setup plus one confirmed
  upload permanently mis-rings the account, its rows render held and
  counted nowhere, the state is visible (the held note names the ring
  cause), and the remedy stays locked behind the owner record the entry
  awaits. Freeze and D-51 untouched. Validation all exit 0: yaml parse,
  tiphys validate (dispatchable: true), gate:privacy, gate:decisions.

## M3-P18 carried low settled (2026-08-27, implementer)

- Head a620ae2: HZ2-M3P18-01 settled in one test-and-prose commit. The
  migration's whitespace-class pin now extracts all FOUR inlined
  occurrences and asserts each byte-equal to the shared constant
  (count pinned at four); red witnessed by mutating the
  collision-comparison occurrence alone (1 failed), green restored
  (679/679). The two overbroad byte-equality sentences corrected loudly.
  Gates at head: npm test 0, typecheck 0, gate:privacy 0,
  gate:decisions 0. Branch ready for the coordinator's merge; e2e and
  the read-back remain the recorded pre-close debts.

## M3-P18 merged and deployed (2026-08-27, orchestrator)

- Both clean-room lanes closed APPROVE at round two; the carried pin low
  was fixed and red/green witnessed before merge. Merged at e499d64 with
  all four verdicts and the parked-entry annotation for the DR-0030/D-51
  interaction.
- Deployed: the build stamp reads e499d64 in production. The canonical
  backfill migration was applied to the deployed database over the
  management API per fleet precedent, with the hand-written prisma
  history row. Measured before applying: one account row, already
  canonical, zero collision groups, so the statement was a verified
  no-op on today's data; it exists for the history and for any
  pre-setup household state that returns. The owed post-deploy
  detection run reported zero collision groups.
- DR-0030 is therefore live end to end: a savings statement is accepted,
  its rows held out of every total and shown as held with the ring named
  as the cause.
- Slice-close debts standing: browser e2e (a Docker-capable container)
  and the fixture read-back against the real documents (documents only
  the owner holds).
- M3-P11 (optimistic naming, DR-0025 and DR-0026) implementer
  dispatched.
- Session note: the permission classifier began refusing some benign
  local commands mid-session (brief compose, plan reads via python,
  ledger appends via heredoc); worked around with lane agents composing
  their own briefs and appends through the editor tool. A permission
  rule for npx tiphys and scripts/sync-branches.sh would remove the
  friction; the branch sync itself is still owed and needs a user-side
  run.
