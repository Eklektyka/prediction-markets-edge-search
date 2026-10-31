# Phase 32 — Slow Convergence Bound

_Generated: 2026-09-20 21:05 UTC_
_Matched games: 830 (soccer, Pinnacle closing from football-data.co.uk)._
_Fit: Jul–Sep 2025. Eval: Oct 2025 – Jan 2026._
_NFL/NBA: SBR data ends 2022, no overlap with PM data (Jul 2025+). Soccer only._

## 1. PM vs De-Vigged Close by Hours to Kickoff

| Hours before KO | N | Median |gap| | P75 |gap| | Share > 4c |
|-----------------|---|-------------|------------|------------|
| 24 | 119 | 1.62c | 3.47c | 19% |
| 12 | 383 | 1.44c | 2.66c | 12% |
| 6 | 446 | 1.17c | 1.98c | 5% |
| 3 | 453 | 1.13c | 1.95c | 5% |
| 1 | 456 | 1.16c | 1.99c | 5% |
| 0.5 | 456 | 2.00c | 9.34c | 35% |
| 0 | 456 | 2.30c | 16.82c | 46% |

**By league (at kickoff, h=0):**

| League | N | Median |gap| | Share > 4c |
|--------|---|-------------|------------|
| Bundesliga | 67 | 8.26c | 57% |
| EPL | 141 | 1.95c | 45% |
| La Liga | 79 | 2.21c | 44% |
| Ligue 1 | 67 | 2.13c | 45% |
| Serie A | 102 | 1.55c | 41% |

**By PM liquidity tercile (at kickoff):**

| Tercile | N | Median |gap| | Share > 4c |
|---------|---|-------------|------------|
| Mid | 181 | 6.28c | 59% |
| High | 275 | 1.46c | 37% |

**Signed gap conditional on cluster-0 net position direction:**

_(Positive = PM overvalues the side c0 is long on → c0 bought too high relative to close)_

| Hours before KO | N | Mean signed gap |
|-----------------|---|----------------|
| 24 | 108 | -0.44c |
| 12 | 362 | -0.17c |
| 6 | 417 | -0.11c |
| 3 | 422 | -0.13c |
| 1 | 423 | -0.19c |
| 0.5 | 423 | -0.07c |
| 0 | 423 | +1.44c |

## 2. Cluster-0 Entries vs Close

**C0 fills on matched soccer markets:** 8555 fills on 423 games

| Hours to KO | N | Mean (fill − close) | Median |
|-------------|---|---------------------|--------|
| 24–99 h | 949 | -0.17c | -0.38c |
| 12–24 h | 671 | +1.27c | +0.91c |
| 6–12 h | 1337 | +1.18c | +1.20c |
| 3–6 h | 1179 | +0.52c | +0.62c |
| 1–3 h | 1418 | +0.45c | +0.18c |
| 0–1 h | 1804 | +1.87c | +0.19c |

## 3. Delayed-Observer Strategy

Enter toward the reference when |PM − ref| > spread (2c) + fee. Exit at kickoff. €200/signal, max 5 concurrent, €2,000 capital.

### Upper bound (close = LOOK-AHEAD)

| Period | Trades | FM% | Mean/trade | CI lo | CI hi | €/month | Months+ | T1–T5 |
|--------|--------|-----|-----------|-------|-------|---------|---------|-------|
| fit | 121 | 100% | €-6.33 | €-31.29 | €+21.50 | €-383 | 1/2 | — |
| eval | 148 | 100% | €+0.90 | €-12.09 | €+12.91 | €+33 | 3/4 | NOT TRADEABLE |

**Monthly series (eval):**

| Month | Net € |
|-------|-------|
| 2025-10 | €-300 |
| 2025-11 | €+194 |
| 2025-12 | €+42 |
| 2026-01 | €+198 |

**T1–T5 (NOT TRADEABLE):** T1 CI>0: FAIL · T2 ≥60% months>0: PASS · T3 concentration: FAIL · T4 latency: PASS · T5 ≥€100/mo: FAIL

**By league (eval):**

| League | Trades | Mean/trade | Total € |
|--------|--------|-----------|---------|
| Bundesliga | 21 | €-3.36 | €-71 |
| EPL | 49 | €-6.16 | €-302 |
| La Liga | 23 | €+7.67 | €+176 |
| Ligue 1 | 24 | €+13.11 | €+315 |
| Serie A | 31 | €+0.49 | €+15 |

### Floor (PM at t−30 min, no look-ahead)

| Period | Trades | FM% | Mean/trade | CI lo | CI hi | €/month | Months+ | T1–T5 |
|--------|--------|-----|-----------|-------|-------|---------|---------|-------|
| fit | 127 | 100% | €-15.29 | €-33.95 | €+3.83 | €-971 | 0/2 | — |
| eval | 91 | 100% | €+12.45 | €-10.79 | €+37.50 | €+283 | 2/4 | NOT TRADEABLE |

**Monthly series (eval):**

| Month | Net € |
|-------|-------|
| 2025-10 | €+1,175 |
| 2025-11 | €-40 |
| 2025-12 | €+30 |
| 2026-01 | €-32 |

**T1–T5 (NOT TRADEABLE):** T1 CI>0: FAIL · T2 ≥60% months>0: FAIL · T3 concentration: FAIL · T4 latency: PASS · T5 ≥€100/mo: PASS

**By league (eval):**

| League | Trades | Mean/trade | Total € |
|--------|--------|-----------|---------|
| Bundesliga | 14 | €-18.72 | €-262 |
| EPL | 30 | €+9.55 | €+287 |
| La Liga | 16 | €+3.93 | €+63 |
| Ligue 1 | 14 | €+56.49 | €+791 |
| Serie A | 17 | €+14.98 | €+255 |

## 4. Verdict

The PM–Pinnacle gap at kickoff is 2.3c (median, N=456). At −3 h the gap is 1.1c and at −1 h it is 1.2c. The gap is roughly constant from −3 h to kickoff — no clear drift. The upper-bound strategy (using the actual close as reference, LOOK-AHEAD) produces €+33/month on the eval period (NOT TRADEABLE). The no-look-ahead floor (PM's own price 30 min ago as reference) produces €+283/month (NOT TRADEABLE). A 15–30 min delayed line feed would sit between these bounds. Even the look-ahead upper bound fails T1–T5, so no delayed feed can work.
