# Deployed infrastructure

Provisioned by the orchestrator with owner approval, 2026-08-17.

- Supabase: project "pulse", ref ygsarzjqosqmkqibqogk, region eu-central-1,
  org ThomasHendrickx (dedicated, per charter; never shared with Hemma).
  Cost approved by owner: 10 USD per month.
- Vercel: project "pulse", id prj_aktwMwqBivpCofVJmaUp7DCy91Qi, linked to
  github.com/ThomasHendrickx/pulse, production branch main, deploys on push.
  No deployment yet: main carries no app until M1-P1 merges.

Still to wire at M1-P1 merge (deploy-verify stage): Vercel environment
variables (Supabase URL, publishable key, database connection string for
Prisma migrations). No secrets in this file, ever.

## Deploy-verify record, M1-P1 (2026-08-17)

Observed by the orchestrator:
1. Deployed URL serves the sign-in page: curl 200 on
   https://pulse-thomas-hendrickxs-projects.vercel.app/sign-in, form markup
   present (server action id, email and password fields).
2. Deployed database schema applied: owner-directed route via the Supabase
   management API (env-variable route not available to the owner). The
   initial migration DDL applied verbatim; the _prisma_migrations baseline
   row written with the migration file sha256
   (d17948e6...aa6d1b) replicating prisma migrate resolve --applied, so
   future prisma migrate deploy sees a consistent history. Verified:
   public.households, public.users, public._prisma_migrations (1 row).
3. Database-backed request through the deployed app: NOT executable from
   this container. Two named causes: the Playwright chromium cannot
   complete TLS through the egress relay for vercel.app (curl can;
   net-log shows the CONNECT sent and TLS dying in-tunnel; CA trust and
   ECH and post-quantum disabling all ruled out), and a scripted
   credential POST is refused by the session permission layer. Witness
   route chosen instead: the owner signs up in their own browser and the
   orchestrator verifies the resulting rows from the data side.

Open advisory surfaced to the owner: Supabase reports RLS disabled on the
three public tables (critical). The app reaches Postgres via Prisma as
table owner, so enabling RLS without policies does not affect it; the
charter names RLS as backstop. Proper enablement belongs in an M1-P2
migration; interim enablement is the owner's call.

RESOLUTION of the RLS advisory (2026-08-17): the owner disabled the
Supabase Data API entirely, matching the pure server-side architecture
(zero client components; Supabase reached only from server actions), so
PostgREST no longer exists as an attack surface and the anon key grants
only the public Auth endpoints. No interim hand-applied RLS: enablement
ships in an M1-P2 migration per the charter backstop clause, keeping the
repo the owner of schema state.

## Deploy-verify debugging record, M1-P1 continued (2026-08-17 late)

Root-cause chain so far: (1) DATABASE_URL initially pointed at the direct
db.<ref>.supabase.co host, which has no IPv4 A record and is unreachable
from Vercel by construction. (2) While fixing it, the owner's env edit
dropped NEXT_PUBLIC_SUPABASE_URL (confirmed by the owner's Vercel log
paste), which broke subsequent deploy behavior and masked the fix. (3)
With vars restored and pooler URL in place, P1001 persists with ZERO
connection arrivals in Supabase's own pooler logs across every probe.
Open hypotheses: Supabase network restrictions dropping at the edge, or
a still-wrong host value. Micro rounds landed: diagnostics logging (PR3),
prisma generate in build (PR4), db health probe (PR6), build-safe env
validation (PR7). Round 4 (env host disclosure on the public endpoint)
was security-flagged and abandoned in favor of a boolean-only triage
probe (round 6, in flight). Process notes: the permission layer treats
agent self-widening of permissions as a hard boundary regardless of chat
approval; the Vercel MCP connector is scoped to three pre-pulse projects
and cannot see pulse (claude.ai-side OAuth scoping).

FINAL TRIAGE VERDICT (2026-08-18, from the deployed function itself):
all four public pooler endpoints (aws-0/aws-1 eu-central-1, 6543/5432)
are TCP-reachable from Vercel, so no network restrictions exist; the
env DATABASE_URL host is a supabase domain with NO IPv4 and is not a
known pooler host (the boolean fingerprint of the direct db.<ref> host
with only the port changed to 6543), and the DIRECT_URL host is not a
supabase domain at all. Root cause: hostname never changed to the
pooler host. Fix delivered to the owner: paste both full URI strings
from the Supabase Connect panel (ref in the USERNAME, pooler host in
the hostname), redeploy. Awaiting owner action; health watch armed.

WATCH PARKED (2026-08-18 13:00Z): deploy-verify health unchanged (same
P1001 fingerprint, poolers reachable, env hosts still wrong). Automated
checks end here per plan; closure resumes when the owner replaces the
two Vercel connection strings with the Supabase Connect panel values
and redeploys. Verification path on resume: /api/health/db ok, then
row verification, then M1-P1 closure.

DEPLOY-VERIFY GREEN (2026-08-18): /api/health/db returns status ok,
db reachable, after the owner set BOTH DATABASE_URL and DIRECT_URL to
the session pooler string (aws-0-eu-central-1.pooler.supabase.com:5432,
ref in username). Root cause final form: this project exposes
transaction pooling on the db.<ref> host, which like the direct
connection is IPv6-only; the session pooler is the only IPv4 endpoint,
so it must serve both variables. Recorded for M1-P2 and all future env
work. Remaining for full M1-P1 closure: the owner completes one
sign-up through the deployed app (the db-backed request witness), then
row verification closes criterion 0.6.

M1-P1 DEPLOY-VERIFY CLOSED (2026-08-18). Criterion 0.6 discharged in
full: deployed URL serves the app (observed), deployed schema applied
with consistent Prisma history (observed), and the database-backed
request through the deployed app completed end to end: the owner
signed up in their browser and the data side shows auth_users=1,
users=1, households=1 with a named household. Every M1-P1 acceptance
criterion is now executed-and-green. The phase that began with the
walking-skeleton dispatch is complete: implemented, dual clean-room
reviewed, fix rounds applied and delta-verified, merged under DR-0012
with DR-0003 arbitration, deployed, and verified against the live
environment with the owner as first real user.

## M1-P2 deploy record (2026-08-19)

- Merge: PR #9 squash, main 12de8f56, landed the PRE-FIX head 10beb40
  by mistake (see backlog tuition 2026-08-19); repaired by PR #10 squash,
  main 6fc43c9, which landed the reviewed head 41ff037's exact tree.
- Migration 20260818154929_import_facts_and_declarations applied to the
  deployed project via the Supabase management API (apply_migration),
  followed by a hand-written _prisma_migrations row with the file's
  sha256 (54668add33caefd56c863dff0646057b55d95528f0773c0ea2fe7dbc3e846473),
  same route as the M1-P1 baseline. All six public tables verified
  rowsecurity=true post-apply.
- Verdicts landed on pulse main as four commits (fcd2c52, 322cfd7,
  634a51b, dcee344) mirroring the M1-P1 pattern; scope amendments
  cfbf722 and d311950 preceded the merge.
- Deploy verification: /import route presence plus /api/health/db ok on
  the production alias (Vercel auto-deploy from main); live browser e2e
  still owner-side only (Playwright TLS relay limitation, see tuition).

## M1-P3 deploy record (2026-08-19)

- Merge: PR #11 squash, main 343dbfc, reviewed head cd788c4 verified
  equal to the PR object's head sha before merging (tuition rule
  applied). Verdicts landed on main as six commits (both lenses, three
  rounds each).
- Migration 20260819152307_transfer_links applied via the management
  API plus hand-written _prisma_migrations row (sha256
  4be95f20cc65b34b463558eb3ceaf3c28ac6f117b5a9a80fe0b091f70dd74663);
  rowsecurity=t verified live.
- Review effort: criteria APPROVE round 0 (zero findings, 13 mutations
  across rounds, three from-zero databases); hazard two fix rounds
  (CR-301 critical settlement window flip, CR-302 re-parse ordinal
  subset, CR-303 window-scoped refunds, CR-304 crash marker, CR-305/6
  lows, then CR-307 medium from its own delta sweep), APPROVE at
  cd788c4. CR-308 low tracked in backlog.

## M1-P4 deploy record (2026-08-20)

- Merge: PR #12 squash, main 05ac512, reviewed head b021f9b verified
  equal to the PR object's head sha before merging. Verdicts on main as
  four commits.
- Migrations 20260820073725_merchants_rules_and_tags and
  20260820084740_one_primary_per_merchant applied via the management
  API with baseline rows (sha256 9c2cbec7..., 0099b846...); live checks:
  rls_tables=11, one-primary partial index present, migrations table at
  5 rows.
- Review: criteria APPROVE zero findings both rounds; hazard three lows
  fixed in a micro round before merge (the tag-primary race measured
  19/20 reproducible pre-fix, 0/20 post), APPROVE at b021f9b. CR-404
  low tracked, owed to M1-P5.
- Incident (fleet-side only): a mid-sequence cd left the shell in the
  fleet repo, landing four pulse verdict commits on pulse-fleet main;
  caught immediately, fleet main reset and force-pushed clean, verdicts
  re-landed on pulse main. Standing rule: repo-mutating sequences pin
  their repo with git -C, never rely on inherited cwd.

## M1-P5 deploy record and M1 milestone closure (2026-08-20)

- Merge: PR #13 squash, main e4ea3ba, reviewed head c7b6507 verified
  equal to the PR object's head before merging; post-merge main diffs
  empty against the reviewed head on all code paths. No migrations this
  phase, deployed schema unchanged at 5 applied migrations.
- Review: three rounds per lens. Criteria APPROVE throughout with
  independent fixture-byte re-derivation and the full DR-0001 chain
  re-executed on from-zero databases each round. Hazard drove two fix
  rounds (CR-501 high: three executed panel-lying constructions, all
  now impossible with the cause partition exact; CR-502/503 medium;
  four lows) and closed the ledger with zero unresolved findings at any
  severity. Nothing rides to the backlog from this phase.
- DR-0001 evidence at the release head: typecheck 0, lint 0,
  gate:tokens 0, 249/249 fast gate, 12/12 e2e, one commit.
- THE M1 WALKING SKELETON IS COMPLETE (slices 0 to 4, five phases, five
  squash merges, every one dual-reviewed under DR-0012/DR-0003).
  DR-0002 now gates M2: the owner uses Pulse for one real month, then
  decides whether slices 5 and 6 still look right.

## M3-P1 closure (2026-08-21)

- Navigation merged twice: PR #14 (7796fc9, the phase) and PR #15
  (155509e, the owner-reported mobile defect round). Verified heads both
  times; the CR-701 audit-trail correction independently re-verified by
  orchestrator grep runs pre-merge.
- Owner feedback loop worked as designed: deployed, owner broke it on a
  phone within hours, criterion amended (1.5 phone viewport), red-first
  fix caught a second latent overflow, standing phone-viewport rule now
  in tuition for all UI phases.

## M3-P2 closure (2026-08-21)

- Merge: PR #16 squash, main a577e51, code head 22f717b verified (docs
  head 960f1f3). No migration. Real-statement parse verified in both
  review rounds; privacy scrubs clean across full history.
- CR-903's chromium e2e crashes root-caused to container disk at 100
  percent; 21GB reclaimed (warning 10), merge witness 19/19 captured on
  healthy disk.

## M3-P2 deploy-verify saga closure (2026-08-21)

Owner-reported /import 500 on PDF upload. Three micro rounds in the
M1-P1 pattern (PRs #17, #18, #19; the first dual-reviewed, the latter
two as observable micro rounds under the deploy-verify precedent):
1. 6775e18: pages decoupled from module availability, /api/health/pdf
   staged-boolean probe, prod-mode smoke in the gate, tracing includes,
   engines pin. Probe stayed red: moduleLoad failed.
2. 971ab57: pdf engine bundled via literal specifiers with the
   pdfjsWorker global short-circuiting pdf.js's computed import;
   external packaging dependence removed, witnessed by deleting the
   package locally. Probe named ReferenceError.
3. c51f95a: DOMMatrix. pdf.js polyfills it from optional native
   @napi-rs/canvas which never ships in a deployed bundle; guarded
   minimal 2D-affine shim, green under Node 18 and no-native Node 26,
   extraction byte-exact. DEPLOYED PROBE GREEN: {"status":"ok"} with
   real server-side extraction.
Standing caveats: engines pin does not visibly control Vercel's
runtime (owner may set Node 22.x in project settings); the app's own
floor is Node 20+ (instanceof File). Escaped-defect mechanism (gate
exercised next dev only) fixed structurally with the chromium-prod
project.
