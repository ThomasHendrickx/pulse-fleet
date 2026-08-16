---
# DR-0012: delegated merge authority for the Pulse fleet.
# Owner-authored grant that assurance-modes.yaml's full mode names in
# granted-by. Validates against schemas/decision-record.schema.json.
kind: decision-record
id: DR-0012
project: pulse
task: fleet-setup
question: >-
  May the orchestrator merge a phase branch without a per-merge owner
  sign-off, and under exactly which conditions?
options:
  - label: Delegate merge authority under the full mode's conditions
    consequence: >-
      The orchestrator merges when every granted-by condition of the full
      mode holds: two independent clean-room reviews on different model
      families, no unresolved high or medium finding, both reviewers walked
      or executed the phase's acceptance criteria, CI green on the exact
      head, the scope audit passes against files-to-touch, and any reviewer
      disagreement is arbitrated with recorded evidence. The owner reviews
      merges asynchronously and can revert; the pipeline does not block on
      owner availability.
  - label: Owner signs off every merge
    consequence: >-
      Every phase waits on the owner. For a solo owner running a pilot in
      spare hours, the pipeline stalls for days at a time and the pilot
      measures owner latency instead of kernel behaviour.
reversibility: reversible
vetoable: true
revert-cost: >-
  Withdraw this record and set merges back to owner sign-off; one commit in
  the fleet home, no code or data affected.
status: decided
decided: >-
  Delegate merge authority under the full mode's conditions (owner,
  2026-08-16). The delegation covers the pulse project only, is bounded by
  the charter's escalation-contract stop-for list, and any merge that would
  touch a stop-for item still halts for an owner decision.
date: "2026-08-16"
evidence:
  - charter/pulse.yaml
---

# DR-0012: delegated merge authority for the Pulse fleet

The question: may the orchestrator merge a phase branch without a per-merge
owner sign-off, and under exactly which conditions?

The decision, by the owner on 2026-08-16: delegate merge authority under the
full mode's conditions. The delegation covers the pulse project only, is
bounded by the charter's escalation-contract stop-for list, and any merge
that would touch a stop-for item still halts for an owner decision. The owner
reviews merges asynchronously and can revert; the pipeline does not block on
owner availability.

The rejected option was owner sign-off on every merge: for a solo owner
running a pilot in spare hours, the pipeline stalls for days at a time and
the pilot measures owner latency instead of kernel behaviour.

This grant is reversible and vetoable. Revert cost: withdraw this record and
set merges back to owner sign-off; one commit in the fleet home, no code or
data affected.

## What "clean" means, defined here so it cannot be softened later

All six conditions must hold for a delegated merge. `assurance-modes.yaml`
quotes them from this record; a condition that is not a whole quoted item
below does not resolve.

- Two independent clean-room reviews exist for the current head, produced on different model families, each written to `delivery/review/` and committed.
- Neither review carries an unresolved finding at high or medium severity. Low findings may be merged with, provided each is either fixed or explicitly recorded as a tracked item with a reason.
- Both reviewers were given the phase's acceptance criteria as their contract, and both walked or executed them. A review that only read is not sufficient for a code phase.
- CI is green on the exact head being merged, not on an earlier one.
- The scope audit passes: changed files are on the phase's files-to-touch list plus the two standing pre-authorized extras.
- Where the reviews disagree, the orchestrator arbitrates with evidence and records the arbitration in the merge commit or in the review file. A disagreement is never resolved by preferring the more convenient verdict.

## Evidence

- charter/pulse.yaml
