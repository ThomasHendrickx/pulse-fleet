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
