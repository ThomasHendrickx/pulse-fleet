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
13. `npx supabase start` fails in this container and the error names the
    wrong component: an rlimit failure is printed right after "Starting
    database from backup", but the process actually asking for a nofile
    limit above the container's hard ceiling of 20000 is the
    edge-runtime container, and `cap_sys_resource` is dropped so nothing
    can raise it. Workaround, found in M3-P7: set `enabled = false`
    under `[edge_runtime]` in supabase/config.toml, start the stack,
    then revert. That file is TRACKED, so revert it before committing or
    the change lands in the repository.
14. NEVER PUT A MECHANICS QUESTION TO THE OWNER (DR-0029). A question
    reaches the owner only when the two answers produce different
    software, different data, or a different risk they would live with.
    How the work is carried out is never theirs: agent count and
    parallelism, worktrees, watchdogs, ports, brief wording, review
    sequencing, retries, whether a stalled agent is resumed or replaced.
    If you cannot name the consequence the owner would feel, there is no
    question, only a decision you have not made yet. The failure this
    records: after the owner interrupted four concurrent agents because
    orchestrator machinery filled every turn and their messages could
    not land, the orchestrator promised to run one agent at a time and
    ASK before running more. That turned a volume problem into an asking
    problem and cost the owner more attention, not less. The answer to
    too much orchestrator noise is less orchestrator noise. Report
    outcomes and decisions the owner would feel; do not narrate the
    machine.
15. A REVIEW'S CONCRETE-EDIT IS A PROPOSAL, NEVER A PATCH. A fix round
    that pastes a reviewer's suggested wording into the plan verbatim is
    adopting text that was written against one criterion in isolation,
    by someone who was not reading its neighbours. Walk every proposed
    edit against the criteria and hazards around it before taking it,
    exactly as you would walk your own sentence, and say in the fix
    round's report which edits you took as offered and which you
    rewrote. The failure this records, found by the reviewer against its
    own work: an M0-P6 round-three concrete-edit was adopted word for
    word and contradicted the criterion eight lines above it, because
    the edit's author had not read that one and the fix round did not
    look. Review edits were entering the plan with less scrutiny than
    the plan's own sentences received. A reviewer is not more right than
    an implementer; the contract only makes them independent.
16. THE AMBIENT DATABASE_URL IN A FLEET CONTAINER IS NOT PULSE. Measured
    2026-08-24: it resolves to the owner's hemma-dev Supabase project,
    with a working password, and no credential for the Pulse project
    exists in the container. Any script that writes and does not pin its
    own target will therefore write to the wrong database and succeed.
    src/platform/db/guard.ts covers destructive commands through
    guard-cli only; it does not cover an ordinary writing routine. Pin
    the connection explicitly for anything that writes, and prefer a
    local database you started yourself. Never run a migration or a
    re-derivation against the ambient connection.
17. DUAL REVIEW MUST BE DECORRELATED IN THE DISPATCH, NOT ONLY IN THE
    CONTRACT. Both M3-P12 round-two lanes were dispatched on the same
    model and the kernel's `dual-review-decorrelation` check failed on
    that dispatch fact, even though the two lanes reached genuinely
    different findings and independently witnessed the same defect. The
    contract split (criteria against hazard) is not decorrelation by
    itself. Vary the model between the two lanes of a dual review when
    dispatching, and record which lane ran on which.
18. THE E2E GATE IS NOT RELIABLY GREEN UNDER FLEET CONTENTION, AND THAT
    IS AN ORCHESTRATOR FAILURE, NOT AN IMPLEMENTER ONE. Measured
    2026-08-24 by a round-three criteria lane: machine load 5.8 to 9.44
    on a four-core box, node, next and playwright processes from at
    least three other worktrees concurrent, `test/e2e/golden-journey`
    producing two DIFFERENT failure symptoms on two consecutive isolated
    runs and passing cleanly in 26 seconds on a third, and a full-suite
    run cascading into ERR_CONNECTION_REFUSED after another worktree
    retook ports 3000 and 3100 mid-run. That is resource starvation, not
    a logic defect. Two consequences. First, never dispatch two lanes
    that will both run `npm run test:e2e` at the same time; stagger them
    and say in the dispatch which one owns the ports. Second, when a
    lane reports a red e2e gate, ask what the load was before asking
    what the code did. `reuseExistingServer` is deliberately false, so a
    lane that moves ports to avoid a clash introduces its own confound
    and must say so.
19. THE LOCAL SUPABASE STACK IS SHARED BY EVERY WORKTREE AND ITS
    PROJECT NAME IS NOT YOURS. `npx supabase stop` acts on the project
    the CURRENT DIRECTORY's config names, and a lane that reverts a
    config file before running cleanup stops whichever project the
    reverted file names, which will be a sibling's. Measured
    2026-08-24: a review lane's cleanup stopped a stack that had been up
    46 hours, with `--no-backup`, and the shared database came back with
    zero tables in `public`. It is recoverable, a dev stack is rebuilt by
    `prisma migrate deploy` and a seed, and the only real loss is the
    time of every lane that had migrated its own database on it. Two
    rules follow. Never run `supabase stop` with `--no-backup` unless you
    started that exact stack yourself in that exact directory and no
    other lane is running. And run cleanup BEFORE reverting any config
    the cleanup command reads, never after.
20. A REVIEW LANE THAT BREAKS SOMETHING MUST SAY SO IN ITS REPORT, AND
    THE ORCHESTRATOR MUST CHECK THE DAMAGE RATHER THAN THE CLAIM. The
    lane in warning 19 disclosed the incident prominently and called the
    damage likely unrecoverable. It was not: the stack was back up and
    the loss was schema that a migrate rebuilds. Disclosure is the
    behaviour to reward; the severity in a self-report is the thing to
    verify, in both directions.
