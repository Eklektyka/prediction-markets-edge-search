## P2 Bug Check + Corrected Rerun

### Bug Check Results

| Check | Result |
|-------|--------|
| 1. Entry before creation | **19,929** of 21,365 entry attempts (93%) had entry time before market creation — excluded |
| 2. I1 at entry | Partial sums only (unfilled outcomes excluded). Mean ~0.15 (most outcomes unfilled at entry) |
| 3. Unfilled legs | 8,529 of 14,891 legs (57%) — removed from both cost and payoff |
| 4. Late-added winners | 0 winners on late-added outcomes |
| 5. I3 tail bins | Weather tail bins ("or higher"/"or below") handled correctly in negRisk |
| 6. Reconciliation | buy + sell: max|diff| = 0.133 — FAILS. Non-zero because sell premium ≠ buy cost when different legs fill at different prices |
| 7. I12 (ann > 50%) | **All cells** except Tech 30d/0.15 have |ann| > 50% → all flagged ⚠BUG |

### Critical Findings

**93% of entries invalid.** The prior P2 result (+442% annualised weather, +294% sports)
was almost entirely entries on markets that did not exist at the stated entry time.
Weather has 3-day median lead time; T-60d and T-90d entries are meaningless.

After filtering to valid entries (market creation before entry):
- **Only 1,436 valid entries** survived (of the original 33,000)
- Event counts per cell: 1–111 (vs 600–1,700 before)
- **No cell has ≥200 eval entries** → T1–T5 cannot be assessed

**Reconciliation fails** because the sell and buy sides use the same fill price
per outcome but the winning outcome's payoff differs: sell pays (1 − fill) on the winner,
buy receives 1 for the winner. When the basket is partial (unfilled legs),
sell premium ≠ buy cost → buy ≠ −sell.

**I12: all cells flagged.** Annualised returns range from −1000% to +3000%, driven by
tiny sample sizes (1–10 events per cell at 60/90d entry) and the denominator effect
(small capital locked → extreme annualisation).

### Corrected Verdict

**The bounded longshot strategy (P2) is NOT TRADEABLE.**
- Insufficient events for T1–T5 at any entry horizon ≥30 days
- Most multi-outcome events are too short-lived for pre-entry
- The prior +442% weather result was 100% entry-before-creation bug
- Politics (T-30d, θ=0.10) has 110 events with net +0.047/event — the most promising
  cell but below the 200-event threshold and with |ann| > 50% (I12 flag)

**What would make it testable:** A universe of politics events with ≥90 day lead time
AND ≥200 resolved events. Currently only 123 politics events have ≥90d lead.
