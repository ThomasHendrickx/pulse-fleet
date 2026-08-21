# M3-P2 fix round 1: consolidated findings

Criteria APPROVE (real Belfius statement parsed through the committed
adapter: 39 rows, balance identity held, deterministic); hazard
FIX-ROUND-NEEDED. Findings: CR-901 medium (declaration gap, resolved by
plan amendment d324182), HZ-001 medium (zero-sum blind spot plus key
poisoning; monotonicity gate fix), HZ-002 medium (inert templateVersion;
fail closed), HZ-003/005/006 low (failure-reason labels, useSystemFonts,
comment overclaim), CR-902 low (confirm table 393px at 390), CR-903 low
(pre-existing month-view e2e chromium crash, moving; structural
mitigation plus a green full run required at the merge head), HZ-004 low
(cross-format duplication, PLAN-PARKED, escalated in the hazard header;
carried to the owner: a household importing CSV and PDF of the same
account-month would double rows; tripwire plan edit recommended).
Privacy: both reviewers' independent history scrubs clean, the leak
rewrite held everywhere including unreachable objects.
