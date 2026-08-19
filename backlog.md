# Backlog

- Kernel 0.1.0 spawn scrubs the child environment (M2-P8 allowlist) and ships no agent adapter (PR-013 deferred), so a real agent payload cannot authenticate under tiphys spawn. The M0-P1 plan-writer was dispatched as a harness agent with the composed brief, incremental output, and a freshness watchdog armed at dispatch. Pilot tuition for the kernel: an agent adapter or a sanctioned per-invocation allowlist extension is needed before spawn can carry real work.

- v0.2 intake landed: delivery/intake/pulse-v0.2-pdf-addendum.md in the pulse repo (PDF import, layout templates, balance contract, mobile share-sheet upload). Plan iteration v0.2 starts from it after v0.1 ships; PR-0xx style planning waits for the DR-0002 checkpoint unless the owner pulls it earlier.

- Kernel 0.1.0 verdict validation in a downstream fleet: the mandated --context . form cannot pass as documented (plan.yaml, work-history.yaml, assurance-modes.yaml are resolved at the context root, where a project keeps them under delivery/ and the fleet root), and dual-review-decorrelation hard-requires two produced-by families, which DR-0003 overrides for this pilot. Both M1-P1 reviewers escalated instead of recording a false family; merge-gate arbitration cites DR-0003. Kernel tuition: the check needs a declared same-family override path, and context resolution needs project-layout awareness.

- Fleet-container tuition: Playwright chromium cannot reach vercel.app through the egress relay (TLS dies in-tunnel; curl unaffected; CA, ECH, ML-KEM ruled out). Production-URL e2e runs need either a relay fix or an owner-side runner. Recorded during M1-P1 deploy-verify.

- M1-P1 CLOSED including deploy-verify (2026-08-18). Next dispatch: M1-P2 (import), which also carries the RLS migration and the two tracked lows CR-006/CR-007 from the M1-P1 review.

## M1-P2 merged-with low findings (DR-0012 condition 2 tracking)

- CR-208 (hazard delta, low): a negative value inside a Debit-headed
  column parses sign-inverted; pre-exists on the two-populated-column
  path, F3 only widened reachability. Fix: fail loud on signed values in
  directional columns. Owed to the next import-touching round (M1-P3 or
  first parser follow-up).
- CR-209 (hazard delta, low): parseSourceProfileSpec accepts
  case-colliding indicator tokens (debitValue "X", creditValue "x"),
  making the credit token unreachable. Fix: reject equal-after-uppercase
  pairs. Same owner as CR-208.
- M1P2-C6 (owner decision pending): second same-format card binds to the
  first card's account; transparency shipped (landing account always
  named), full fix needs an owner call between per-upload account choice
  when a household holds 2+ accounts on one profile spec, or
  file-carried card identity once M1P2-C1 verifies real CSV layouts.
