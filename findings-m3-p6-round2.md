# M3-P6 fix round 2: consolidated findings

Hazard delta APPROVE (three lows carried). Criteria delta
FIX-ROUND-NEEDED with one high, one medium, two lows. Both reviewers
independently verified the round-1 work is otherwise sound: CR-01
closed (0 of 5 preview rows show the number, guard falsifiable),
CR-02 hoist closed, masking measured 23 of 23 card-labelled
descriptors with 0 of 16 unlabelled rows touched, keys 27 to 25 with
exactly two merges that lose no distinguishing word, dedup keys still
byte-identical, gates 338 unit and 21 e2e.

| id | sev | finding |
|---|---|---|
| CR-06 | high | KEY-SPACE CLOSURE REGRESSION on the real file: normaliseCounterparty(key) != key for 10 of 39 rows at the fix head, versus 8 at BOTH the base and the round-0 head. The pipeline erodes one four-digit group per application (P4 P4 P4 W to P4 P4 W to P4 W). Cause isolated to one line: POSTAL_CODE_BEFORE_CITY now consumes the code alone where the base consumed code AND following word. Driven end to end: submitted subject differs from stored pattern, rowsResolvedByTheRule = 0, every total still right. This is hazard H6.4, the exact hazard the implementer invoked to reject HZ-08. |
| CR-06b | high (same fix) | The new closure invariant is the right invariant and is under-tested: it asserts the fixpoint over the FIXTURE's groups only, and the fixture contains no run of four-digit groups before a word, so it is green at 338 while the property is false on 10 real rows. An invariant that reads only the fixture measures the fixture. |
| CR-07 | medium | The test named "the SQL rule and the TypeScript rule agree on every value Prisma can produce" evaluates no SQL: the reviewer set the SQL fragment to the OPPOSITE precedence, updated both pin literals as a pin update would, and got 22 passed. The pins catch an accidental edit; nothing compares the two rules. |
| CR-08 | low | The surface derivation does not reproduce (7 sites for the reviewer, not the recorded 8; the cited lines are now a date cell and a comment because adding the masking call moved the counterparty cell out of its own derivation) and it is blind to multi-line JSX, the direction that hides a NEW unmasked surface. The gap-row masking added this round has no guard at all. |
| CR-09 | low | Narrowing the masker from digit shape to card-label grammar is right on re-measured evidence, but it changes what criterion 6.3's closing clause asserts and is not among the five declared deviations. |
| HZ-09 | low | The dash-token drop introduces a new same-class collapse (distinct at e0704a5, equal at 2f8b62d), unreachable on real data, unpinned. |
| HZ-10 | low | The declared residue is understated: a card number behind a NON-DUTCH label (French, English) is equally unmasked AND unstripped, so it lands in the key and in a stored rule. Unreachable via PDF today (French fingerprints as layout-unsupported) but reachable via the generic delimited path, and Pulse ships French copy. |
| HZ-11 | low | False test name at normalise-counterparty.test.ts:86 ("stripped wherever it sits"), now untrue mid-string; the body only covers the trailing case. |

Judgement recorded by both lenses: the implementer's HZ-08 dispute was
CORRECT and both load-bearing steps were independently verified, and
the hazard reviewer additionally implemented its own proposal and
confirmed it breaks closure. That dispute stands.
