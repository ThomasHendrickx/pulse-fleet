# M1-P2 fix round 1: consolidated findings

Source: dual clean-room review at head 10beb40. Criteria verdict
(worktrees/m1-p2-review-criteria, 66d42e3) FIX-ROUND-NEEDED, hazard verdict
(worktrees/m1-p2-review-hazard, 5d4fa88) FIX-ROUND-NEEDED. Both reviewers
converged on the top finding independently. Deduped across contracts.

| # | From | Sev | Finding |
|---|---|---|---|
| F1 | CR-201 + CR-008 | high | Second same-format card silently ingests into the first card's account (spec-equality profile binding, upload-statement.ts:104-109, confirm-import.ts:75-82); identical cross-card tuples silently dropped because the hash key carries the misbound accountId. Facts immutability makes it unrepairable in-product. The criteria contract itself forces the binding (card re-upload asks nothing), so the full fix is a contract question for the owner; the in-round fix is transparency only. |
| F2 | CR-202 | medium | Unknown or empty D/C indicator is signed CREDIT (parse-statement.ts:193-197). Probe: STORNO and blank stored positive. Never guess a sign. |
| F3 | CR-203 + CR-009 | medium | Debit/credit-pair export with an empty credit column this month detects as signed and proposes every debit POSITIVE (hazard H1.1 realized outside the fixture matrix). |
| F4 | CR-204 | medium | Racing confirms of one awaiting import double-ingest under two ghost accounts; no status guard (confirm-import.ts:39-41 read-then-act, import-repository.ts:201-210 unconditional). Probe: 3-row file stored as 6 rows across 2 accounts. |
| F5 | CR-205 | low | Occurrence ordinal counts natural-keyed siblings, contradicting the comment at dedup.ts:50-51 (R-087) and duplicating a keyless row across overlapping imports (ordinal #1 in file A, #0 in file B). Addendum section 5 recipe binds. |
| F6 | CR-206 | low | contentTuple unescaped "\|" join lets two distinct transactions collide on one dedup key. Recipe declared frozen; no production transactions exist yet, so the fix window is NOW. |
| F7 | CR-207 | low | `keys[index] ?? ""` in both use cases converts a future array desync from a crash into silent multi-row loss on the empty dedup key. |
| F8 | CR-010 | low | False present-tense comment on unused export countTransactionsForAccount (import-repository.ts:215-216), no caller exists (R-087). |

Cleared by review, no action: H1.2 (mixed-account check precedes every write,
ingest transactional), H1.3 phase half (rawLine verbatim, zero mutation paths),
deployed RLS vs session-pooler role (same role migrates and serves, owner
bypasses RLS; wrong-ownership failure is a loud migrate error, not silent).

Owner decisions raised alongside this round: F1 full fix (per-upload account
choice when a household holds 2+ accounts on one profile spec, or file-carried
card identity once M1P2-C1 verifies real CSV layouts).

Disposition: all eight to the same implementer (R-061), findings applied
before further execution (R-030). Delta verification after the fix commits.
