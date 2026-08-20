# M1-P5 fix round 1: consolidated findings

Dual clean-room review at head d21ec71 (code af759b3). Criteria verdict
APPROVE with one low; hazard verdict FIX-ROUND-NEEDED with four
findings. Id collision: both reviewers minted "CR-501"; the criteria
reviewer's low is relabelled CR-505 here and everywhere downstream.

| # | Sev | Finding |
|---|---|---|
| CR-501 | high | The reconciliation verdict is difference===0 and nothing else, while the difference includes matched INTERNAL legs whose partner books outside the month. Executed: (a) a settlement pair straddling month end alarms with difference 850,00 and ZERO cause blocks, two months running, recurring whenever direct-debit lag crosses month end; (b) cancelling gaps flip the verdict to "Books close" over real export gaps (the CR-301 class at the presentation layer); (c) "Books close" renders above a listed UNRESOLVED gap. Plus an R-087 false comment at month-projection.ts:157-160. Fix: matched-internal filter, render the in-transit cause line, ok verdict requires empty causes. |
| CR-502 | medium | flow is nullable and ingest commits before interpretation's separate transaction: a crash between them leaves durable rows invisible to every surface including rowCount and changeInPot, panel green (probe: byte-identical figures with a committed 777,00 flow-NULL row). Only marker is a transient redirect param. |
| CR-503 | medium | PULSE_FIXED_NOW is honoured under NODE_ENV=production (executed probe): a leaked var silently freezes the month view. isProduction() sits unused in the same file; config comment claim unchecked (R-087). |
| CR-504 | low | The pure projection layer (foldGroups, deriveMonthFigures, month.ts) has zero fast-gate tests; year-boundary comparison has no executed gate coverage (reviewer probes exercised it green). |
| CR-505 | low | playwright.config.ts reuseExistingServer: true can silently reuse a stale dev server from another fleet worktree without the pinned clock (criteria reviewer; port was verified free for its runs). |

Cleared by probe: tenancy complete in every query, single-negation sign
consistency, integer cents end to end, no new Transaction write paths,
token and locale gates with teeth, UNRESOLVED reachability claim
confirmed by 12-shape classifier battery, month arithmetic across year
boundary, leap February and Brussels DST edges.

Disposition: all five to the same implementer (R-061). CR-501 touches
the reconciliation identity's PRESENTATION, not its definition: the fix
closes the identity exactly by accounting for in-transit legs as a named
cause; if the implementer judges any part changes the identity's meaning
rather than its rendering, stop and escalate per the charter stop-for
list.
