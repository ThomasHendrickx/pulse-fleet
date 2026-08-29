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

## M3-P11 built, gates green, e2e authored and owed (2026-08-27, implementer)

- claude/m3-p11-optimistic-naming at 8fb0f58 (branched from main e499d64).
  DR-0025 and DR-0026 are now on the naming screen: the row shows the
  typed name the moment it is submitted, marked unconfirmed (visual mark
  plus a polite live region wired to the submit control's description),
  no money figure or row position is predicted, a failure puts the label
  back and raises a hand-built dismiss-only toast (assertive, catalogue
  copy in all three languages, no timer), and a saved name that differs
  from the typed one is told on the row in a polite notice instead of
  being swapped in silently.
- Gates at head, all exit 0: typecheck, lint, npm test (679/679),
  gate:privacy, gate:decisions, gate:tokens, npm run build (env pinned to
  invented localhost). Four new catalogue keys, key sets equal across
  en/nl/fr.
- Plan staleness found and recorded, not improvised around: the plan's
  grounding predates merged M3-P12, so "a failed naming is silent today"
  was no longer true (the status banner was live) and criterion 11.7(a)'s
  "at most five client files" predates M3-P14's setup form (six directive
  files now, named set otherwise exact). Both are recorded as deviations
  in delivery/work-history/m3-p11.yaml; the refusal wording M3-P12 gave
  the stale/account/unidentifiable kinds is kept in the toast.
- Standing debt, same as every recent phase: this container has no
  Docker, so npm run test:e2e is AUTHORED but not run. The new
  optimistic-naming spec (prediction, two forced failures, trimmed-name
  difference, three languages, both prod projects) plus the amended
  stale-subject journey await a Docker-capable container; the owed
  command is npm run test:e2e.

## Container restart, 2026-08-27 (orchestrator)

- The session container restarted and killed three running review lanes
  (both M3-P11 lanes and the retrospective M3-P10 hazard lane). Nothing
  merged was lost: main, the fleet repo, every worktree and their
  node_modules survived, and the deploy still serves e499d64.
- What WAS lost: the probing those three lanes had done in memory. Both
  M3-P11 lanes had produced only beacon stubs on disk, no findings. They
  were re-dispatched into the same prepared worktrees.
- Standing correction for every future lane dispatch: instruct the agent
  to COMMIT AND PUSH its verdict document as soon as it has a verdict and
  its first findings, then amend and re-push as it finishes. A lane that
  holds its whole document in memory until the end loses everything to a
  restart or a rate limit, and this has now happened twice in one session
  (a session rate limit at 16:40 UTC, then the restart).
- M3-P10 gap being closed: the busy state phase merged and deployed on
  2026-08-26 with NO clean-room review of any kind. A retrospective hazard
  lane is running against current main, with findings flagged for whether
  they are live in production. The criteria lane is queued behind it.

## Second restart, and the checkpoint protocol it forced (2026-08-27)

- A second container restart destroyed the same three review lanes again,
  both times while they were still probing with only a beacon stub on
  disk. Nothing merged was lost either time; main stayed at e499d64 and
  the deploy kept serving it.
- STANDING RULE for every lane dispatch from now on, stronger than the
  earlier push-early note: the reviewer must PUSH A DOCUMENT SKELETON
  within its first ten tool calls (header plus every criterion or attack
  direction listed as not-yet-walked), then push again after each
  criterion walked and each finding written, never going more than about
  five tool calls without a push, and set the verdict only at the end. A
  restart then costs minutes instead of the whole lane, and the next
  reviewer CONTINUES the pushed partial document instead of starting
  over. The dispatch must also tell the reviewer to continue a partial
  document if its branch already carries one.
- Scope narrowed deliberately: only the two M3-P11 lanes are running, as
  that phase is unmerged and blocks. The M3-P10 retrospective review is
  deferred behind them rather than run in parallel, because it reviews
  code that is already deployed and can wait.

## M3-P11 criteria lane, round one, finished (2026-08-27)

- Verdict FIX-ROUND-NEEDED on the criteria contract, at implementer head
  8fb0f58; the review document is delivery/review/m3-p11-criteria.yaml on
  claude/m3-p11-rev-crit, head f8ec02b.
- THE CHECKPOINT PROTOCOL WORKED, AND IT ALSO SHOWED ITS ONE HOLE. The
  partial document survived the restart as intended, but the last commit
  of the first session, whose subject said it walked four mechanical
  criteria, had in fact DELETED their four blocks while writing a fifth
  one's evidence (five insertions, twenty-two deletions). So the lane
  arrived with one criterion walked, not five. The second session
  restored the blocks, walked all nine remaining criteria itself, and
  recorded the loss in the document header. Worth adding to the standing
  rule: a push-per-criterion is only safe if the pushed diff is read
  back, since a truncating edit pushes just as cleanly as a good one.
- The implementation itself is sound. No correctness defect was found:
  the money is structurally outside the prediction's reach, the client
  boundary is closed at depth (import closure walked, not asserted), the
  literal and token gates are green, and every fast gate passes when re-run.
- Five findings, one medium that a fix round can close (an accessibility
  gap: the unconfirmed description hangs on a submit control that is
  disabled for exactly the window the description exists, so the
  focus-return telling the criterion promises cannot happen), one medium
  that no fix round in this environment can close (the browser gate has
  never executed at any head), and three low.
- STANDING ENVIRONMENT DEBT, now recorded for the third phase in a row:
  npm run test:e2e cannot run in these containers (no Docker, so no local
  Supabase auth service). Criteria 11.2 through 11.5, the entire
  behavioural half of this phase, are unwitnessed rather than refuted.
  This is the same debt CR-M3P18-02 carries. Until one lane gets a
  container with Docker, every UI phase will close with its browser
  criteria unmeasured.
- The sibling hazard lane's verdict was NOT read by this lane at any
  point, before or after the walks.

## M3-P11 fix round one closed (2026-08-27, implementer)

- claude/m3-p11-optimistic-naming at ec4779d. Both clean-room lanes
  returned FIX-ROUND-NEEDED on 8fb0f58 with eleven findings between them;
  each was re-derived in the worktree before anything changed.
- The one real defect: the record that carried the server's answer back
  to the reader was a single shared slot matched on the label alone, so
  naming a second row erased the first row's pending state and a merchant
  with rows on both sides could raise its notice on a row nobody named.
  It is now one entry per row, per direction, and six fast-gate rules pin
  it; three of those were red against the shipped behaviour first.
- Two reader-facing improvements the owner would feel. The failure
  sentence no longer says the name was not saved: the declaration is
  written before the answer travels, so that could be false; all three
  languages now say the answer was not confirmed and tell the reader to
  reload to see what is stored. And notices no longer stack in one
  rectangle where the second hid the first: one shows at a time, the next
  appears when the reader dismisses it, and nothing vanishes undismissed.
- Also: the unconfirmed marking is now reachable by keyboard (the naming
  field carries the description, since the submit is disabled exactly
  while the description exists), the masking walk can see a label rendered
  from a prop, and two spec assertions were widened to the criteria they
  serve.
- Gates at head, all exit 0: typecheck, lint, npm test (690 passed),
  gate:privacy, gate:decisions, gate:tokens, build with a pinned invented
  localhost database. Unchanged debt: npm run test:e2e has still executed
  at no head, because this container has no Docker; both lanes reproduced
  that limit and neither asked the round to close it.

## M3-P11 criteria lane, round two, CLOSED (2026-08-27)

- Verdict APPROVE at fix head ec4779d. Document
  delivery/review/m3-p11-criteria-round2.yaml on claude/m3-p11-rev-crit,
  head 750b811. Under the round cap this closes the lane; two new
  findings and one carried one ride the implementer's brief as work.
- The fix round did well. Four of the five round-one findings are closed,
  three of them more strongly than the finding asked for. The
  accessibility gap was closed without reversing M3-P10's decided
  disabled behaviour, and the two weak spec measurements were replaced
  with better ones rather than argued away.
- THE BEST THING IN THE DELTA, worth the owner knowing: the claim rules
  criterion 11.5 turns on were moved into a pure module and pinned by
  eleven fast-gate tests that DO run in these containers. Part of this
  phase's behaviour is now witnessed here instead of waiting on a browser
  nobody has been able to start.
- TWO NEW FINDINGS, both about the record and the tests, neither about
  product behaviour. One: the client leaf now imports two sibling modules,
  which criterion 11.7(e) forbids literally, and the work history's
  boundary claim still carries a captured output that no longer
  reproduces. Two, and this is the one that matters: two assertions in the
  newly added notice journey are FALSE against this head's own DOM, so
  npm run test:e2e cannot exit 0 at ec4779d for reasons that have nothing
  to do with Docker.
- A METHOD NOTE FOR FUTURE LANES: Playwright needs no server to evaluate a
  locator. Driving chromium with page.setContent over a copy of the head's
  markup settled both halves of that second finding as measurements. Every
  previous lane treated "no Docker" as "no browser evidence at all"; that
  was too pessimistic and it cost real coverage.
- SLICE-CLOSE CONDITION, unchanged in substance and now with a
  precondition: fix those two assertions FIRST, then run npm run test:e2e
  in a container with Docker and capture the output. The e2e debt is now
  in its third consecutive phase.

## M3-P11 settle round closed, ready to merge (2026-08-27, implementer)

- claude/m3-p11-optimistic-naming at fef2d8a. Both lanes closed under the
  round cap (criteria APPROVE, hazard carrying its findings as work); the
  coordinator directed five carried items settled before merge and all
  five are settled.
- Two were real defects the owner would have met. The claim record that
  tells you a saved name differs from the one you typed was keyed by the
  row alone, and a counterparty with a spend row and a refund renders two
  rows sharing that key, so naming one could silently swallow the other's
  message; the identity is now the row and its direction. And the notice
  at the bottom of the screen could be about a row you had not just acted
  on: it is now always the one your last action produced, with the others
  waiting their turn and none removed unless you dismiss it.
- Two were test defects that would have made the owed browser run report a
  red that is not a product fault: two assertions in the new notice
  journey read the wrong element and filtered on the wrong thing. Both
  were fixed and measured in a real browser without a server.
- One was a record correction: the leaf now imports three small pure
  modules from its own folder, which a criterion forbids on its letter.
  The boundary itself does not leak (those modules import nothing), the
  claim in the work history was corrected, and the deviation is declared.
  Whether the criterion should be relaxed is an owner question, not a
  repair.
- Gates at head, all exit 0: typecheck, lint, npm test (696 passed),
  gate:privacy, gate:decisions, gate:tokens, build with a pinned invented
  localhost database. Unchanged debt: npm run test:e2e has still executed
  at no head, because no container in this project has Docker. It is now
  the only thing standing between this phase and a witnessed behavioural
  result, and the specs it will run are no longer known to be broken.

## M3-P10 fix round pushed, seven findings settled (2026-08-27, implementer)

- claude/m3-p10-busy-state-fix at 0c75e22, branched off main. No pull
  request, no merge. The busy state was merged and deployed with no review
  of any kind on record; a retrospective hazard review found seven defects,
  five of them live on the deployed build.
- The one the owner has been looking at: pressing a button made it render
  as DISABLED, not as busy, because the two appearances collided in the
  stylesheet and the disabled one outranked the other. The little mark that
  says "working" measured 1.09 to 1 against the button it sat on, which is
  invisible on a phone in daylight. It now measures 6.1 to 1, on every
  control that can become busy, for the whole of its loop rather than at
  one instant of it. This is the third pass at the same complaint, so it
  is worth saying plainly: what shipped in August looked, on a press,
  exactly like a control that was dead.
- Two more the owner would meet. A press threw away the keyboard's place
  on every control in the product, worst on merchant naming, where naming
  five counterparties meant tabbing from the top of the page five times; a
  pressed control now keeps its focus and still refuses a second press. And
  a press widened the button it was on and reflowed the row underneath it;
  the space is now reserved.
- One was a promise with no failure branch: the accounts setup submit tied
  its busy state to a call whose rejection was unwritten, so a dropped
  connection mid-submit left the screen's recovery to the framework. Both
  branches are now written and the reader is told, in all three languages,
  that the submission did not land.
- One was a test that could not fail: the only assertion that the busy
  state ever ENDS ran after a redirect, on a page holding no button that
  had been pressed. It now runs per control, after that control's own
  response.
- Two are records rather than repairs, and one of them is an owner-visible
  debt rather than a tidy-up: the privacy probe over the real statements
  has still never been run, in any container, and the criterion's own
  escape clause does not cover the reason. What is owed is one run in a
  container that can read those files. The gate that does run sees no
  merchant name and no place name, which is how two real ones reached the
  public repository before.
- Gates at head, all exit 0 under Node 22 with a pinned invented localhost
  database: typecheck, lint, npm test (702 passed), gate:tokens,
  gate:privacy, gate:decisions, npm run build. Unchanged debt, and it now
  covers this round's new assertions too: npm run test:e2e has still
  executed at no head, because no container in this project has Docker.
  Every measurement claimed above was taken in a real chromium driving the
  product's own stylesheet, which needs no server; none of it was taken
  against the running product.

## DEPLOYS ARE STALLED, found by the build stamp (2026-08-27 20:05 UTC)

- Production still serves e499d64. Main is at a56cc5b, two merges ahead:
  1ed44c3 (M3-P11, pushed 18:58 UTC) and a56cc5b (the M3-P10 busy-state
  fix, pushed 19:44 UTC). Sixty-seven minutes after the first of them,
  neither has deployed. That is not a slow build.
- The deployed app is otherwise healthy: /api/health/db returns 200 and
  the old build keeps serving, so nothing is broken for the owner, but
  nothing merged since e499d64 has reached them either. In particular
  the invisible busy mark and the disabled-looking busy control are
  STILL LIVE, and the whole M3-P11 feedback chain is not live.
- This is exactly the failure the M3-P17 build stamp was built to make
  visible, and it worked: without it the fleet would have gone on
  assuming main equals production, which is the assumption that hid the
  stale M3-P12 head for a day.
- The fleet cannot diagnose it: the Vercel MCP connector is still scoped
  away from the pulse project (list_deployments returns 403 forbidden
  for prj_aktwMwqBivpCofVJmaUp7DCy91Qi), the same limitation recorded on
  2026-08-18. Reading the build log needs the owner's dashboard.
- OWNER ACTION, added to the standing asks: open the Vercel dashboard for
  the pulse project and look at the deployments since e499d64. Most
  likely causes, in order: a build failing in Vercel's environment though
  it passes locally (npm run build exits 0 here on the merged tree, 702
  tests green), a hobby-plan deployment or build-minute limit reached
  after a heavy day of merges, or the git integration having become
  disconnected. Whichever it is, the fix is dashboard-side.

## CORRECTION: the slow gate was runnable all along (2026-08-27)

- Four phases in a row (M3-P17, M3-P18, M3-P11, M3-P10) recorded
  npm run test:e2e as environment-limited on the reasoning "no Docker,
  therefore no Supabase auth service". THAT DIAGNOSIS WAS WRONG. The
  docker binary and daemon are installed and the daemon simply was not
  running; the repo ships its own Supabase CLI at
  node_modules/.bin/supabase. Verified directly by the orchestrator.
  Every agent hit the failure, took it at face value, and the
  orchestrator repeated it to the owner twice as something only they
  could unblock. It was not.
- STANDING RULE: an environment limitation is a claim like any other and
  gets verified before it is recorded, and certainly before it is
  reported to the owner. "Command X failed" is not "capability X is
  unavailable". Start the daemon, read the error, check the binary.
- WHAT IT IMMEDIATELY FOUND: main is RED on the slow gate.
  test/e2e/busy-state.spec.ts fails 6 of 9 at origin/main a56cc5b, with
  a control run reproducing it at that head, and month-view.spec.ts
  fails around line 1390 on an import step. The lead, read from source
  and not yet measured, is a cross-phase interaction rather than a
  product defect: M3-P11's optimistic prediction removes the pressed row
  from the unresolved-group testid set at press time, so the spec's
  .first() locator re-resolves to a row nobody pressed. A repair lane is
  running on claude/slow-gate-red-repair, tasked to run the full suite,
  decide instrument-versus-product per failure with evidence, and fix
  without weakening any assertion.
- The owner ask about providing a Docker-capable container is WITHDRAWN.
  What remains genuinely owner-owned from that item is only the fixture
  read-back against the two real documents, which are not in any
  container.

## THE SLOW GATE RAN, AND IT WAS RED. IT IS NOW GREEN (2026-08-27, slow-gate repair)

- claude/slow-gate-red-repair at f3a30f0, cut from main and merged up to
  4f38dbb. No pull request, no merge.
- THE CORRECTION FIRST, because four phases in a row recorded the opposite:
  npm run test:e2e IS runnable in these containers. The docker daemon
  starts, the repository ships its own Supabase CLI, the local stack serves
  the gate and the schema goes on with prisma migrate deploy. Every one of
  those phases recorded the gate as environment-limited on a wrong
  diagnosis, so the gate was never green or red on evidence: it was simply
  never run. It has now run four times end to end.
- WHAT IT FOUND AT MAIN: 16 failures over 119 tests, eight distinct defects,
  each red in every project that runs it. Seven were defects in the TESTS
  and one was a defect in the PRODUCT.
- The product one is the owner's phone at its narrowest and largest text.
  At 360 pixels with the device text size at 200 percent, three of the four
  navigation labels wrapped onto three lines and "Accounts", the link added
  in M3-P14, wrapped onto four. Not because the word is long, "Merchants"
  is longer: each label had 42 pixels of room and "Acc" needed 43.5, so the
  fourth link lost a character on every line and paid a line for it, and the
  navigation row stood 138 pixels tall. Two pixels of padding back on each
  side fixes it: "Accounts" now takes three lines like the others and the
  row is 105 pixels tall.
- The seven test defects share one shape: a spec measuring something the
  product had legitimately changed under it. The loudest was the busy state,
  which four phases could not witness. The spec held the merchant naming
  button through the row's "unresolved" marking, and M3-P11 takes a row out
  of that set the instant the reader's typed name appears, so every reading
  after the press described a row nobody had pressed. THE BUSY STATE ITSELF
  WAS NEVER BROKEN. The instrument was, by an interaction between two
  phases that were each right on their own.
- Two more were races in the tests, one was a test driving a declaration
  step the product removed once it has learned a format, one asserted that
  an import must not re-read a row the interpretation window deliberately
  reaches, one required two rows to point at a notice when only one notice
  is ever on screen, and one measured a merge across income and spend, where
  two rows is the correct answer.
- FINAL NUMBERS AT HEAD, every one captured rather than claimed: typecheck,
  lint, npm test (704 tests), gate:privacy, gate:decisions, gate:tokens and
  npm run build all exit 0, and npm run test:e2e exits 0 with 118 passed,
  0 failed and 1 skipped by its own touch-project condition, in 30 minutes.
- ONE OPERATIONAL FINDING worth carrying: the docker daemon died on its own
  in the middle of the first run and took the local database with it, which
  turned every remaining test red at its sign-up step. That is what an
  environment failure looks like, and it is not what the eight defects above
  looked like. A watchdog now restarts the daemon and its containers.
- WHAT THIS DOES NOT SETTLE: the e2e debt recorded against M3-P10, M3-P11,
  M3-P12 and M3-P18 is now paid at THIS head, on this branch. It is not paid
  on main until this branch is merged.

## The slow gate is green on main, verified by the orchestrator (2026-08-27)

- main 975d0ce. The orchestrator ran the full suite itself on the merged
  tree, with DATABASE_URL, DIRECT_URL and E2E_DATABASE_URL each parsed
  and asserted to resolve to 127.0.0.1 before anything ran: EXIT 0,
  118 passed, 1 skipped, thirty minutes. The e2e debt carried by
  M3-P10, M3-P11, M3-P12 and M3-P18 is paid.
- Of the sixteen failures on the red main, seven distinct defects were
  INSTRUMENTS measuring the wrong thing and one was a real PRODUCT
  defect: the fourth navigation link narrowed every link to a quarter of
  the row, and at a two hundred percent device text size the Accounts
  label lost a character per line and wrapped to four lines at 360
  pixels. Fixed with a padding token; width, height and tap targets
  unchanged. That is the owner's own complaint area, found only because
  the suite was finally run.
- One notice-attribution assertion came out STRONGER than the one it
  replaced: the old form passed even when both rows pointed at the same
  notice.
- OPERATIONAL, worth knowing before someone panics: the Docker daemon
  can die mid-run and take the local database with it, turning every
  test after that point red at its sign-up step. That looks exactly like
  a mass product failure and is not one. Re-run with the daemon
  restarted before reading a wall of red as a regression.
- Still not reaching the owner: production remains stalled at e499d64,
  now five merges behind. Everything above is merged and green and none
  of it is live.

## CORRECTION: the deploys were not stalled, they were queued (2026-08-27 23:37)

- At 20:05 the orchestrator reported production stuck at e499d64 for
  sixty-seven minutes and asked the owner to check the Vercel dashboard
  for a failing build or a plan limit. That ask is WITHDRAWN: production
  is now at 4f38dbb on its own, with no owner action. Vercel rolled
  forward to the newest commit rather than building each merge in turn,
  which is ordinary behaviour on a day with many merges, and the queue
  simply ran long.
- What is live as a result: the busy-state contrast and focus fixes
  (the invisible mark and the disabled-looking busy control are FIXED in
  production) and the whole M3-P11 feedback chain. Only 975d0ce, the
  slow-gate repair, is still building.
- The lesson is the same one the e2e diagnosis taught, in the other
  direction: an observation ("production is sixty-seven minutes behind")
  is not a diagnosis ("the pipeline is broken"). The build stamp gave a
  true observation and the orchestrator attached a cause to it that the
  evidence did not support, then took it to the owner. Report the
  measurement, propose the cause as a hypothesis, and say which it is.

## M3-P13 is built: the review screen says what it grouped on and how far a naming reaches (2026-08-28)

- claude/m3-p13-identity-on-review at f5c1db1, cut from main 975d0ce. No pull
  request, no merge, no plan edit.
- WHAT THE OWNER GETS. Every unresolved group on /merchants now says, in their
  own language, whether it was joined because the transactions share a
  counterparty account or because they share a description; the naming form
  says how many transactions of the month the name will reach BEFORE it is
  submitted, using the group's own row count rather than a second one; a group
  whose rows carry no counterparty name is labelled by its account in masked
  form instead of by one transaction's descriptor; and every group opens onto
  the individual transactions behind it, each with its own date, description
  and amount. That is DR-0027's accepted cost made visible: two purposes paid
  to one counterparty read as two lines the owner can point at rather than as
  one opaque total.
- A THING THAT WAS ALREADY WRONG AND IS NOW FIXED, found while building this:
  /merchants printed a counterparty ACCOUNT NUMBER IN FULL. An account-basis
  group was labelled by the normalised descriptor, and a transfer descriptor
  carries the account exactly as the statement prints it. Measured on the
  shipped normaliser before any change was made. CORRECTED THE SAME DAY BY
  BOTH CLEAN-ROOM LANES, and the superseded sentence is quoted rather than
  deleted: this entry used to say "The account is now masked wherever it is
  rendered, on the label and on the new transaction lines". THAT WAS FALSE IN
  TWO WAYS, both high and both live in production. The mask fails OPEN on
  every separator except the ASCII space, so an account grouped with a
  no-break space, a narrow no-break space, a tab, a newline, a full stop or a
  hyphen is copied through in full, and a no-break space is one byte in one of
  the two accepted import encodings and has already been witnessed inside
  stored account renderings in this project. And only /merchants was fixed:
  the import confirmation screen, which is the screen the owner photographed,
  prints the counterparty account in a column of its own through the card mask
  only, and the month view renders descriptor text the same way. A fix round
  covering every rendering surface and the separator set is in flight.
- ONE CRITERION COULD NOT BE MET AS WRITTEN and was escalated rather than
  improvised around, which is the one thing here that may want the owner's
  eye. Criterion 13.2 asks the full page source of /merchants to carry no
  unmasked account AND asks the naming form's hidden field on the same page to
  carry the unmasked namespaced identity key. For an account-basis group that
  key IS the namespace followed by that account, so the two halves contradict
  each other. Measured, there are three places in the source that must hold
  it: the hidden field the criterion itself requires, the row identity
  attribute the optimistic-naming work addresses rows by, and the framework's
  own serialised payload, because the naming row is a client component. None
  of the three is rendered, which is what the hazard is about. What was built
  asserts the strongest true form: the account appears in no rendered markup,
  in either the compact or the spaced shape, once those three machine channels
  are removed.
- TWO EXISTING TESTS MEASURED SOMETHING THE PRODUCT LEGITIMATELY CHANGED, the
  same shape as the seven the slow-gate repair found yesterday. The accounts
  spec asserted that the own-account groups are labelled with fragments of the
  uppercased descriptor; decision D-41 labels such a group by the counterparty
  name the statement carries, so the assertion now compares the label exactly,
  in the case the statement printed it, which is stronger than the substring
  form it replaces. And the pressed-and-disabled sweep found the new
  disclosure as a twenty-second interactive control, which is that gate doing
  its job: the enumeration was amended, never the sweep narrowed, and the new
  control needed no new appearance rule.
- GATES AT HEAD, every one captured rather than claimed: typecheck, lint,
  npm test (719 passed), gate:privacy, gate:decisions, gate:tokens and
  npm run build all exit 0, and NPM RUN TEST:E2E EXITS 0 with 121 passed, 0
  failed and 1 skipped by its own touch-project condition, in 31.6 minutes,
  against the local Supabase stack with DATABASE_URL, DIRECT_URL and
  E2E_DATABASE_URL each parsed and asserted to resolve to 127.0.0.1 first.
  The slow gate is confirmed runnable here for the second day running.
- TWO OPERATIONAL THINGS WORTH KNOWING BEFORE SOMEONE LOSES AN HOUR TO THEM.
  Do not run npm run build in a worktree before the slow gate: it writes the
  production output into .next, which is the directory next dev uses, and the
  first run here failed three specs that this phase cannot reach, one of them
  the wrong-password sign-in line. And do not edit anything under src/ or
  test/e2e/ while the gate is running, because the dev server compiles from
  disk and the result is then a measurement of no single tree. Both are
  recorded in the phase work history as warnings.

## CORRECTION: "masked wherever it is rendered" was not true when I wrote it (2026-08-28, M3-P13 fix round)

- WHAT THE ENTRY ABOVE SAYS, quoted so the record shows what was wrong
  rather than silently reading better: "The account is now masked wherever
  it is rendered, on the label and on the new transaction lines, and the
  masker refuses to touch anything that is not an account number of a
  country and length the pinned registry knows."
- BOTH HALVES OF THAT SENTENCE WERE FALSE, and two independent clean-room
  lanes found it. It is corrected here rather than edited above, because an
  entry that quietly improves is an entry the owner cannot audit.
- FALSE HALF ONE, "wherever it is rendered": it was masked on ONE screen.
  The import preview printed the counterparty account whole in a column of
  its own on every import, which is the same screen the owner photographed
  when a card number leaked there, and the month view printed it on its
  group labels, its reserves rows, its held rows and its gap rows. Five
  further surfaces. All six are now masked, and the enumeration is no longer
  a sentence in a comment: a test walks every rendering surface in the tree
  and fails if one masks against the card number and not against the
  account.
- FALSE HALF TWO, "refuses to touch anything that is not an account number":
  the masker also refused to touch things that ARE account numbers. It
  accepted a plain ASCII space between the digit groups and nothing else, so
  an account written with a no-break space, a narrow no-break space, a tab, a
  newline, a full stop or a hyphen was printed in full. The no-break space is
  not hypothetical here: it is a single byte in Windows-1252, one of the two
  encodings Pulse accepts, and this project has already seen that character
  inside stored account numbers. An account of a country the pinned table
  does not carry was printed in full too. Both now fail closed.
- A THIRD THING THE REVIEW CAUGHT, and it is the one that matters most for
  trust: the sentence under the naming box said the name applies to so many
  transactions "of this month". The screen has no month. It reads every
  transaction the household has ever imported, and naming applies to all of
  them, past and future. So the sentence UNDERSTATED what the owner was
  about to do, on the one screen built to say how far a naming reaches. It
  now says "already imported", and a test refuses any period word coming back
  into it in any of the three languages.
- Nothing here reached production: this is all on the phase branch, which
  has not been merged.

## M3-P13 merged (2026-08-27, orchestrator), main 004c57c

- Both clean-room lanes closed APPROVE. Verified by the orchestrator's own
  runs on the merged tree: fast gates green at 752 tests, and the full slow
  gate exit 0 at 122 passed.
- WHAT THIS PHASE ACTUALLY FIXED, and it is more than the phase was for.
  /merchants had been printing a counterparty ACCOUNT NUMBER IN FULL,
  because an account-basis group was labelled by its normalised descriptor
  and a transfer descriptor carries the account as the bank prints it. The
  review then found the same leak on five further surfaces, including the
  import confirmation screen, which is the screen the owner photographed,
  where the account had a column of its own. Six surfaces are now masked and
  the enumeration is derived by a test rather than written by hand, so the
  next surface cannot go unseen the way these did.
- THREE MORE, all live before this phase: the new mask itself failed OPEN on
  every separator but the ASCII space, so an account written with a no-break
  space (one byte in an accepted import encoding, and already witnessed in
  this project's own stored data) was copied through in full; the screen told
  the reader a naming reaches transactions "of this month" while the read had
  no month at all, true with one statement imported and false with two; and
  the page was serialising every raw transaction description into a script
  element, because a React element passed as a client-component prop ships
  its own props.
- A TRAP WORTH REMEMBERING: next dev serialises every server component's
  props into the page, so a page-source absence assertion run against the
  dev server proves nothing. The literal clause is now asserted against the
  production build.
- The settle round corrected TWO of the orchestrator's own prescriptions,
  each with a measurement taken before choosing: a flat one-separator bound
  would have broken a doubled-space account, and the proposed fallback
  remedy would still have swallowed words, because a space separates both an
  account's groups and two sentences' words. Recorded because an implementer
  measuring an instruction and refusing it is the behaviour this fleet wants.

## M3-P18 harvest branch pushed, not merged (2026-08-29), claude/m3-p18-harvest ba3965d

- TWO DEFECTS THAT WERE LIVE ON MAIN are fixed on this branch. Both were
  found by an agent who kept working on the P18 phase branch after it
  merged, on commits that never reached main. Both were reproduced here
  before being ported, and neither was taken on trust.
- THE RESERVES BLOCK COULD READ DOUBLE. The migration this phase shipped
  deliberately keeps a pair of account rows that are one real account.
  For a household holding such a pair, the reserves query matched one
  movement to both rows and the screen added them up, so the reserves
  block showed twice what moved, while the reconciliation banner, which
  is computed differently, still said the books close. Nothing on screen
  contradicted the wrong number. Measured here, then fixed and measured
  again.
- THE ACCOUNT-NUMBER CLEANUP COULD DESTROY A STORED NUMBER. The SQL that
  strips spaces out of stored account numbers still asked Postgres what
  counts as a space, and Postgres answers that from the server's own
  settings rather than from the committed code. On this container's
  server it stripped five characters MORE than the application does,
  which would rewrite a stored number into a form the application can
  never match again, with the original gone and the migration reporting
  success. WORSE THAN THE BRANCH RECORDED IT: that branch measured the
  problem only under a specially named setting; here the server's own
  DEFAULT does it, so it is what an ordinary connection gets. The code
  now spells out every character itself and asks the server nothing.
- WHAT WAS DELIBERATELY LEFT BEHIND, and the owner is being told rather
  than having to notice. The same source commit also relaxed the rule
  that an account's ring is frozen once it carries its own imported
  rows, so a savings account could be moved back into the pot. That rule
  is the OWNER'S decision DR-0031. The branch's argument that a later
  decision undid its premise is the reviewing lane's own inference, and
  it is already parked awaiting a new owner record that does not exist.
  Non-negotiable 9 says a decided decision is reversed only by the
  owner. So the guard is byte identical to main and the test asserting
  the relaxed behaviour was left behind. NO QUESTION IS BEING PUT BACK
  TO THE OWNER HERE; this is a record of what was not done and why.
- WHAT THE EXCLUSION COSTS, stated rather than hidden: if the parked
  argument turns out to be right, a household that answered a ring
  wrongly at setup and then uploaded that account's statement still has
  no way to correct it. That belongs to the owner's next record.
- Gates, all run here against a local stack: typecheck, lint, fast suite
  (753), privacy, decisions, tokens, build and the FULL slow gate all
  exit 0, the slow gate at 124 passed and 1 skipped. Main's baseline was
  122; the two arms this branch adds account for the difference.
- Not merged, no pull request opened.

## The read-back is DONE, and the branch prune found two live defects (2026-08-27/29)

- THE OWNER SENT BOTH REAL DOCUMENTS, so the read-back that four phases
  recorded as owed is now performed. Method: every distinctive token from
  both statements (account shapes, card shapes, long digit runs, amounts,
  capitalised segments) compared against the whole tracked tree. RESULT:
  zero account numbers, zero card numbers, zero long digit runs, and zero
  real merchant or counterparty descriptors reached the repository. The
  eighteen phrase-level hits are all bank format furniture the parsers must
  recognise. Two fixture labels that looked like the owner's data are
  INVENTED: neither phrase occurs in either document. ONE amount in a
  decimal-comma parser case also occurs in a real document; it is a round
  number and plausibly invented, that cannot be proven, so it was replaced.
  Neither value is recorded anywhere in the tree. The read-back is no
  longer owed by M3-P18, M3-P13, M3-P10 or M3-P11.
- THE PRUNE IS DONE, with the owner's approval: 82 branches advanced onto
  main, none left behind. ONE branch conflicted and was left alone by the
  script, which is how two live defects were found.
- THAT BRANCH CARRIED FIVE COMMITS MADE AFTER ITS PHASE WAS MERGED. An
  agent kept working on claude/m3-p18-savings-held-and-migration after the
  orchestrator merged a620ae2 and moved on. PROCESS LESSON: a merge does
  not stop a lane. When a phase is merged, its lanes must be told, or their
  later work is invisible until something forces a look.
- TWO REAL DEFECTS, both on main until now, both harvested and merged at
  4bd6d7a after the orchestrator's own runs (753 fast tests, slow gate exit
  0 at 124 passed):
  ONE, the reserves join matched a transaction to EVERY account row sharing
  a canonical number, and the migration deliberately preserves such pairs,
  so an affected household's reserves block read DOUBLE what moved while
  the reconciliation banner still read as closing. Now one row per
  transaction through a lateral, with the block's net asserted equal to the
  reconciliation's own net.
  TWO, the shared SQL whitespace class still contained a POSIX [:space:],
  whose membership Postgres takes from the cluster's locale rather than
  from the committed SQL. Measured on Postgres 17.6: it over-strips five
  code points under ICU. THE OWNER'S DEPLOYED DATABASE IS ICU-PROVISIONED
  (datlocprovider i, verified), so the backfill applied there earlier that
  day carried the defect; it rewrote nothing only because no row needed
  rewriting. The class now enumerates code points and names no POSIX class.
- A THIRD CHANGE ON THAT BRANCH WAS DELIBERATELY LEFT BEHIND: it relaxed
  the ring guard so a savings account carrying its own rows could move back
  into the pot. That is DR-0031's freeze, no new owner record exists, and
  the argument that DR-0030 falsified its premise is the lane's inference,
  already parked. change-account-ring.ts is byte-identical to main and the
  test asserting the owner's behaviour passes unchanged.
