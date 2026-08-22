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
11. HARD RULE, owner 2026-08-22: no data of any kind goes into a commit
    message. No amount, no counterparty, no date from a row, no account
    or card number, not even an invented one. A message says what
    changed and why. It never carries a sample of the data, because a
    reviewer reading a message cannot tell an invented figure from a
    real one, and that is exactly how the M3-P3 leak survived three
    pushes. The same applies to prose in reports, verdicts and notes.
    Invented values live in fixtures only, and every account or card
    number in the tree is listed with its provenance in
    test/fixtures/allowed-identifiers.txt. `npm run gate:privacy`
    enforces both halves, joins the standing gate line in CLAUDE.md, and
    fails on any identifier that is not on that list, so a value taken
    from a real statement stops the build rather than reaching a review.
12. THE PULSE REPOSITORY IS PUBLIC and the owner has decided on
    2026-08-22 (DR-0024) that it stays public. Everything you commit is
    world-readable the moment it is pushed, and history keeps it after
    any later correction. Two real merchant descriptors, naming a shop
    and a parking location taken from a real statement while drafting a
    design mockup, reached it that way and were found by BOTH clean-room
    lanes on M3-P3, not by any gate. `npm run gate:privacy` cannot see a
    merchant name, a place name, a date or an amount inside a file,
    because those look exactly like invented ones; its own header now
    says so. Before you commit anything you wrote while looking at a
    real document, read it back line by line and check every string
    yourself. Checking one file of a set and generalising is exactly how
    this got through.
