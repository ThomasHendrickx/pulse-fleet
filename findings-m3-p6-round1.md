# M3-P6 fix round 1: consolidated findings

Both verdicts FIX-ROUND-NEEDED at head e0704a5. Criteria: all ten
criteria MET by execution, and the phase still ships its own target
defect on a screen no criterion names. Hazard: the masking helper does
the wrong job on real data. The two highs interact and must be fixed
together, in this order.

## What is confirmed working, by execution on the owner's real file
- Grouping fixed: 15 card rows, 15 distinct keys before, 6 after
  (groups 4/4/3/2/1/1); every merge inspected is the same merchant at
  the same location; two real sibling branches stay distinct.
- Zero of 15 keys and zero of 15 rendered group labels retain a
  window-length digit run (longest remaining run 6).
- Dedup keys byte-identical base vs head: 160 fixture keys md5
  101fd61b1599a56a0659bddc036be47b both sides, and the real statement's
  keys md5 a7bd4025599ca39b53647372975c37d4 both sides.
- Masking containment holds: helper reachable from 7 UI files only;
  naming a merchant submits the UNMASKED subject and resolves 5 rows
  across 2 months; a masked subject would match 0 of 13.
- KBC unaffected: 22 rows, 16 keys before and after, zero changed; no
  file overlap with the paused M3-P3 branch.
- Both pre-existing phone-viewport defects reproduced and fixed
  (merchant review 462, month view 424).

## Findings, consolidated
| id | sev | finding |
|---|---|---|
| CR-01 | high | The confirm preview (profile-confirmation.tsx:101) renders the raw descriptor: 3 of 5 preview rows show the full card number at 390x844 on the shipped head. This is the screen the owner photographed. H6.6 names it; criterion 6.3's scope (group labels) cannot see it. Declared in extras at 77da0c6. |
| HZ-01 | high | maskCardNumbers does the wrong job on real text: it masks 0 card numbers and 6 NON-card identifiers (IBANs, mandate references, phone numbers) across both statements, joining two numeric fields across a space and eating characters out of alphanumeric tokens. Its own comment at mask-card-number.ts:41 asserting it "can never join two distant numbers" is falsified by probe (R-087). |
| HZ-02 | medium | The non-destructive floor (normalise-counterparty.ts:273) puts the full descriptor, card number included, into a stored rule pattern. Zero real rows reach it; no test covers it. |
| HZ-03 | medium | Rail-prefix wildcard contradicts the file's own pin-never-widen rule at :124; a new collapse demonstrated. |
| HZ-04 | medium | The amount strip is global rather than card-scoped and mangles the space-grouped thousands form into a truncated number. |
| HZ-05 / CR-02 | medium | Third implementation of the merchant-source rule in raw SQL (overview-repository.ts:95 and :266). Both reviewers reached it independently. The month view's grouping reads the SQL copy; 6.10's TypeScript pin is structurally blind to it; the present-tense agreement comment is R-087 shape. Judged an honest scope call AND a real hole. |
| HZ-06 | medium | Cross-rail split on 2 real merchants: the noise list carries an English contactless token occurring in no fixture and no statement, while the Dutch token it needs appears in four fixtures; the "grounded in the committed fixtures" comment is false. |
| CR-03 | low | month-view.tsx:263 renders the unnormalised gap-row descriptor unmasked on the screen this phase masked. |
| CR-04 | low | maskCardNumbers misses a run with a DOUBLE separator. Latent today, load-bearing once CR-01 points the helper at raw text. |
| HZ-07 | low | 6.3's named exception is vacuous on the label side (implementation masks the label). |
| HZ-08 | low | Latent city-collapse once the card tail no longer shields the trailing-city loop. |
| CR-05 | low | npm ci fails under npm 10 / Node 22 (missing @swc/helpers lock entry) while package.json pins engines 22.x. NOT introduced by this phase, probed not to threaten the deployed build (no workflow runs npm ci). Base-branch chore. |

## Escalation carried from the hazard header
Criterion 6.2 witnesses non-collapse on one fixture pair only; two
collapse paths this phase introduces sit outside that pair's reach, and
a collapse writes one stored EXACT rule over two merchants, which no
total reveals. Not reachable on observed data.
