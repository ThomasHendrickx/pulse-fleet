# M1-P3 fix round 1: consolidated findings

Source: dual clean-room review at head f274d30. Criteria verdict APPROVE,
zero findings (worktrees/m1-p3-review-criteria, 3686e95). Hazard verdict
FIX-ROUND-NEEDED (worktrees/m1-p3-review-hazard, 6a4edce), six findings,
all probe-executed. The split is the contracts working as designed: every
acceptance criterion is met while the hazard lens found window-scoping
defects the criteria do not witness.

| # | Sev | Finding |
|---|---|---|
| CR-301 | critical | Settlement totals computed from the window slice, not the card import (interpret.ts:58 over the date-bounded load). A later import's padded window loads a settled debit without its card rows and flips INTERNAL to SPEND, deleting the link; books still reconcile while spend double counts every card month under the normal upload cadence. The plan's own spec says the match is against the card IMPORT's settlement total, so this is implementation-to-spec alignment, not a semantics change. |
| CR-302 | high | Re-parse recomputes hash-path ordinals over the import's STORED SUBSET, not its file. Overlap households: unchanged-spec re-parse leaves duplicate keys in the fake (which lacks the unique index its header claims) and aborts against the real adapter, permanently denying the H2.5 repair. Two R-087 false comments (fix-profile.ts:87, import/application/ports.ts:127). |
| CR-303 | medium | Refund outgoing-history check is window-scoped: same row is SPEND under recompute and INCOME under a later import's window. The plan's refund rule is scope-free. |
| CR-304 | medium | applyReparse commits corrected facts, reinterpretation runs in later transactions, imports stay INTERPRETED: a death between them leaves corrected facts under stale flows with no marker (upload path has INGESTED, this path has nothing). |
| CR-305 | low | Indicator branch Math.abs's an explicitly signed cell (sibling of CR-208's class). |
| CR-306 | low | Settlement matching not exclusive per card import: two equal-amount pattern debits both go INTERNAL onto one import; an unimported card's settlement can vanish from spend on an exact-cent coincidence. |

Cleared: H2.2 (reserve precedence), H2.3 (pairing determinism, tenancy,
per-leg uniqueness), H2.4 (identity holds; its structural blindness to
category flips recorded inside CR-301). No Transaction write paths beyond
flow update, applyReparse, ingest createMany; no delete path. CR-208 and
CR-209 verified closed by probe.

Disposition: all six to the same implementer (R-061). CR-301 and CR-303
are alignment to the plan's written spec. CR-306's exclusivity is an
orchestrator-approved tie-break rule in the D-7 style (one settlement
debit per card import, deterministic choice, losers fall through to
SPEND); if the implementer judges it changes the correction's MEANING,
stop and escalate per the charter stop-for list rather than implementing.
