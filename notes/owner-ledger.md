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
