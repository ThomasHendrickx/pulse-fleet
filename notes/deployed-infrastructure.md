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
