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
