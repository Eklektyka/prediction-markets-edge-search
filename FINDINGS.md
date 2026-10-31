# Findings Ledger

## Candidates Under Evaluation

| # | Strategy | Verdict | Details |
|---|----------|---------|---------|
| A | Candidate A — withheld | **Parked — holdout not completed** | Two candidates remain under evaluation; their markets, rules and results are withheld until out-of-sample testing is complete. |
| B | Candidate B — withheld | **Parked — holdout not completed** | See above. |

## Dead Strategies

| # | Strategy | Verdict | Key Number | Source |
|---|----------|---------|------------|--------|
| 1 | Weather METAR impossible-bin | **Closed — real but sub-viable** | ~$44/month at 25% capture after 3 bug fixes | [`weather-reports/steps_6_7_corrected.md`](weather-reports/steps_6_7_corrected.md) |
| 2 | 15-min crypto fair value | **Closed — null** | Zero first-mover fills | — |
| 3 | Stale-quote sniping | **Closed — null** | One wallet = 85% of activity | [`closures/CLOSURE_chess.md`](closures/CLOSURE_chess.md) |
| 4 | Following sharps' first fill | **Closed — null** | No price move to follow | — |
| 5 | Cross-venue line lag | **Closed — null** | Within 1–2¢ of reference | [`closures/CLOSURE_cross_venue.md`](closures/CLOSURE_cross_venue.md) |
| 6 | Longshots (pooled) | **Closed — null** | Capital-dead | — |
| 7 | Longshots (fee venue) | **Closed — null** | Negative after fees | — |
| 8 | Crypto threshold short vol | **Closed — null** | Negative | — |
| 9 | Multi-outcome basket | **Closed — null** | Bug, then too small | — |
| 10 | Rewards-only | **Closed — sub-viable** | $8–40/month at $2K | — |
| 11 | Young markets | **Closed — null** | — | — |
| 12 | Thin-market maker | **Closed — null** | — | — |
| 13 | Boxing | **Closed — null** | — | — |
| 14 | Fading behavioural biases | **Closed — null** | Biases absorbed at the quote | — |
| 15 | Coherence pairs | **Closed — null** | — | — |
| 16 | Cross-venue arbitrage | **Closed — null** | N=39 matches, 10 signals | [`edge-reports/phase13_tradeability.md`](edge-reports/phase13_tradeability.md) |
| 17 | Resolution scoring | **Closed — null** | Rubric did not discriminate | — |
| 18 | Fee-venue weather | **Closed — null** | 29 fills/yr | — |
| 19 | Line shopping | **Closed — blocked** | Missing per-book data | — |

## Adjacent Studies

| # | Study | Verdict | Key Number | Source |
|---|-------|---------|------------|--------|
| S1 | Order-flow reversal | **Closed — confirmed out of sample (registered holdout, 2026-07-14), economically sub-fee** | Q5−Q1 = −1.13¢, t = −6.29, p < 0.0001; round-trip cost ~5.5¢ | [`closures/CLOSURE_track_b_ofi.md`](closures/CLOSURE_track_b_ofi.md) |
| S2 | Macro drift | **Closed — killed at N=6** | Sign disagrees across versions, both inconclusive | — |
| S3 | Copy-trading feasibility | **Closed — 7/7 gates failed, $0 deployed** | Copy edge negative | [`copy-trading/CLOSEOUT.md`](copy-trading/CLOSEOUT.md) |

## Withheld

| # | Item | Status |
|---|------|--------|
| W1 | Loss anatomy / who-profits analysis | Withheld — paper in preparation |
