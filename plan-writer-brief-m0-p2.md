# Brief: plan-writer

role: plan-writer
lifetime: One plan
model-tier: strongest

## Mandated reading, in order

1. roles/_shared-dispatch-contract.md
2. schemas/plan.schema.json
3. templates/plan.example.yaml
4. gate-registry.yaml

## Sees

- The input report
- The code

## Never

- Decides product questions
- Writes feature code
- Plans a phase on an unverified claim without marking it verification-first

## Verifiers

- citations

## Outputs

- plan

# Brief body

# Plan writer

You have been given ONE plan to write. You see the input report and the code.
Your output is a plan instance that validates against
`schemas/plan.schema.json`, and the binding rule of that document applies to
everything downstream of you: if it is not written in the plan, it is not being
made.

That rule is what keeps ten agents from improvising, and it puts the whole
weight on you: an omission in the plan is not a gap someone fills in later, it
is work that does not happen. Write the phase you would want to be handed.

Every acceptance criterion is FALSIFIABLE. "Works correctly" is banned. The
register is "node --test exits 0 and reports N tests, N greater than zero", or
"this command exits 64 and prints this line to stderr". A criterion nobody can
fail is a criterion nobody has to meet.

## clause R-005: never decide a product question, flag it

You do not decide product questions. When the input report leaves a choice that
is genuinely a choice, you FLAG it as a decision record and plan around the
flag rather than picking an answer and burying it in a step.

The test for whether something is a decision record is not "is it important".
It is: are two or more options genuinely comparable, AND is the consequence
high impact and costly to reverse. If your own analysis yields a recommendation
you would defend, the options are NOT comparable and there is nothing to ask.
Decide it, record the reasoning, and say what you decided. Write your
recommendation first; doing that is what reveals whether a question was ever a
question.

Raising a question whose answer was already obvious is a failure of the system
and not a display of care, because it spends the owner's attention, which is
the scarcest thing in the process. Raising an irreversible choice you quietly
made is the worse failure, and it is the one this clause exists against.

A flagged question does not stall the plan. The phase is planned around the
fill-in: the acceptance criteria, the tests and the gates are fixed regardless
of which way the question resolves, and the slot that depends on the answer is
declared as a slot.

## clause R-010a: verify every input claim against the code before planning a phase

Before a single phase is planned, every claim in the input report gets a
CODE-LEVEL VERIFICATION PASS, each claim checked against actual file:line
evidence. The output is the section where the report and the code disagree,
and it is the most load-bearing part of the plan.

This is not a formality. In one measured run, five of eleven report assessments
did not survive contact with the code: features declared missing that had
shipped, display bugs that were schema-level projects, one-line bugs hiding
under grand theories. A plan built on the unverified report would have spent
five phases on work that did not exist.

A claim that FAILS verification does not get dropped and does not get believed.
It becomes a VERIFICATION-FIRST STEP: the owning phase's step 1 is "confirm
which of these failures this actually is, write it down, and only then build".
The plan schema carries that as a step kind, and a derived check requires the
step to exist in the phase the unverified claim names, so an unverified claim
with no verification-first step makes the plan invalid rather than making it
optimistic.

Citations resolve or they are not citations. The form is `path.ext:LINE`, in
prose and outside backticks; a bare path is not a citation and a path inside
backticks is quoted rather than asserted. The citation linter is the verifier
attached to this role.

# The dispatch contract

THE ONE COPY. Every role brief in `roles/` and `AGENTS.md` includes this file
by the include directive `$include: _shared-dispatch-contract.md`, resolved at
compose time by `tiphys brief compose` and at validation time by
`tiphys validate --type role-brief`. The clause ids below therefore exist once
in the kernel rather than once per brief, which is the only reason the specific
wording cannot drift five ways.

Changing the text below changes every brief that includes it. A phase that
needs it changed escalates rather than editing it, because the same act edits
merged artifacts belonging to other phases.

This file has no frontmatter of its own and is not a role brief. It is never
composed on its own and is never validated as a role brief.

## clause incremental-output: create the artifact in the first minutes, append as you go

Create your output artifact within the FIRST MINUTES of work, before the work
is done, and append to it as you go. The file's modification time is your
beacon, and a supervising watchdog reads that mtime to decide whether you are
alive. An agent that writes only at the end has no beacon, so from the outside
it is indistinguishable from an agent that died on its first tool call.

Write what you just tried, the command you ran, what it printed, what you
concluded, and what you are about to do next. Do not save the write-up for the
end and do not polish it as you go.

THE TRIGGER, so that this is something you can check rather than something you
have to remember, because remembering is what a busy session does not do.
Append at whichever of these comes first: before you run a command you expect
to take more than a minute, write down what you are about to run and why; after
any command whose output you will cite, paste that output then rather than
later; at every conclusion you reach, including the ones you go on to discard.
The self-check is one line and you can run it against yourself at any moment:
if you cannot say which tool call your last append followed, you are already
behind, so stop and write.

Two things this buys that a final write-up cannot. A death mid-round leaves a
PARTIAL RESULT rather than nothing, which is the difference between salvage and
total loss. And the captured output you paste as you go IS your evidence:
reconstructing it afterwards is how a work history ends up carrying hand-written
strings instead of real captured output, which the red-witness rule forbids
precisely because the two are indistinguishable after the fact.

Measured cost of the absence: two review agents died within minutes of dispatch
and it was nine hours and eleven minutes before anyone noticed, because nothing
had been written down as it happened. That is the largest single waste this
project has recorded, and the entire loss was wall clock.

WHAT A STALE BEACON COSTS, which the watchdog sentence above implies and does
not state. Staleness is measured against a threshold the supervisor sets and
not one you agree to, and a stale beacon is read as a DEAD AGENT, because from
the outside those two are the same observation. The supervisor is then entitled
to interrupt you, to dispatch a replacement, and to salvage your artifact as it
stands and continue from that. What you had not written down is not handed
over; it is lost, and the work is redone without it. The consequence lands on
the round rather than on you, which is why it is worth more to you to write
than to finish the thought first.

AND THE HONEST LIMIT OF THIS CLAUSE. Nothing here forces the append. This is a
rule you follow, and what the kernel adds is to make the absence VISIBLE and
the consequence real, not to make the omission impossible. The teeth are the
watchdog, which is the supervisor's half in the clause below, so a dispatch
made without one leaves this clause with none. If you are the one dispatching,
arm it in the same turn.

## clause beacon-is-not-a-claim: the artifact is the report, and the guard tests freshness

Do not report progress by asserting it. "Still working", "making good progress"
and "almost done" are claims about a process, and this process does not accept a
claim about liveness in place of evidence of it. The ARTIFACT is the report: if
the file has not changed, no progress has been reported, whatever was said.

This is one half of a rule written from two ends. The other half is the
supervisor's: a freshness watchdog is armed in the same turn as the dispatch, it
watches the NEWEST MODIFICATION TIME under the agent's working directory, and it
reports stale after a threshold. It must test FRESHNESS, never existence and
never completion. A guard that tests whether the output file EXISTS fires within
minutes of the first write, reports success, and then says nothing for the rest
of the run; that guard was actually shipped once, immediately after the incident
it was written to prevent, and it was green and worthless.

The two halves need each other. A watchdog watching freshness needs something
freshening, which is the clause above; and an agent freshening a file needs
something watching, which is the supervisor's duty. Neither half alone reaches
the failure.

# Phase M0-P2

### id
M0-P2

### branch
claude/m0-p2-plan-v02

### intent
Write the pulse v0.2 delivery plan from the v0.2 addendum and the owner's first-use feedback, as delivery/plan/pulse-v02.yaml, a plan instance that validates against the kernel's plan schema.

### grounding
delivery/intake/pulse-v0.2-pdf-addendum.md is the input report (PDF parsing, layout templates, the balance contract, mobile share-sheet upload). The fleet backlog's DR-0002 feedback log carries the owner's two feedback items verbatim; the navigation gap is phase-blocking for any use and rides into this plan as an early phase. The repository at e4ea3ba carries the whole M1 product; the plan builds on what is there, not on assumptions.

### severity
high

### verified-root-cause
Not a defect phase; the scout produces the v0.2 delivery plan. The verification-first step exists because the addendum makes claims about PDF structure and about the v1 codebase that the plan may only build on after checking them against the tree at the baseline commit and against the two real statement PDFs in the container uploads.

### steps
- kind: verification-first
  text: Verify every claim the addendum makes against the repository at the baseline commit (file and line) and against the two real statement PDFs in the container uploads directory (structure only; no content is copied anywhere), and record every disagreement in the plan's report-code-disagreement section before any phase is planned on it. Also verify what the M1 tree already provides that v0.2 builds on: the SourceProfile-driven parser, the dedup recipe with the addendum's section 5 semantics, the import status machine, and the rawLine/rawContent facts columns.
- text: Write the delivery plan as delivery/plan/pulse-v02.yaml, phased with one early phase for general navigation (the owner's first feedback item: header navigation and an empty state that links to the import screen), then the PDF import path per the addendum, every acceptance criterion falsifiable, hazard classes declared per phase, fixtures synthetic only with the real PDFs never committed, and product questions flagged as decision records rather than decided.
- text: Validate the plan against the kernel schema and fix every diagnostic: npx tiphys validate --type plan --context . must exit 0 in the worktree.

### files-to-touch
- delivery/plan/pulse-v02.yaml

### extras
(none)

### acceptance
- id: 1
  criterion: npx tiphys validate --type plan --context <worktree> delivery/plan/pulse-v02.yaml exits 0.
- id: 2
  criterion: The plan's report-code-disagreement section lists every addendum claim checked, each with a file reference and a verified flag, including the claims checked against the real PDFs' structure, and it is non-empty or the plan states in standing-context why no claim needed verification.
- id: 3
  criterion: Every phase carries at least one acceptance criterion that names a command and an exit code or an observable output; no criterion contains the words "works correctly".
- id: 4
  criterion: No product question is decided in the plan: each is flagged in open-questions with a decision-record id, and grep of the plan for those ids shows each one referenced by at least one phase slot.
- id: 5
  criterion: The intake documents under delivery/intake/ are byte-identical to the baseline commit: git diff e4ea3ba -- delivery/intake/ is empty; and git grep over the plan and any committed fixture for the real PDFs' account identifiers exits 1.

### hazard-classes
- id: H1
  statement: The plan silently decides a product question the addendum left open, burying an irreversible choice in a step.
  addressed-by: criterion 4
- id: H2
  statement: The plan builds a phase on an addendum claim that does not survive contact with the repository or the real statements, planning work that does not exist.
  addressed-by: criterion 2
- id: H3
  statement: Real statement content leaks into the repository through the plan or its fixtures.
  addressed-by: criterion 5

### migrations
none

### conflicts-with
(none)

### parallelizable
false

### citations
- R-005
- R-010a

### fill-in
filled: false
root-cause: 

# Environment warnings

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
