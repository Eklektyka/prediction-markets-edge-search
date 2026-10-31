## B2 — Sports Moneyline at Kickoff

**Book:** football-data.co.uk Pinnacle closing (2654 games, 5 leagues, 2 seasons 2024-25 & 2025-26).
**De-vigging:** three-way multiplicative. Vig range [1.0265, 1.0778].
**PM matching:** 9016 parsed soccer events → 738 matched to FD by team alias + date ±1d.
**Match rate:** 738/2654 = 27.8%.
**NBA leg:** STOPPED — kaggle moneylines end Jan 2023, no overlap with PM trades (start Jul 2025).

### Gap Distribution (PM kickoff price − de-vigged Pinnacle)

| Metric | Value |
|--------|-------|
| N (outcome-level) | 628 |
| N (events) | 348 |
| Mean gap | -0.0017 |
| SD | 0.0469 |
| Within ±2c | 73.6% |
| Within ±4c | 91.9% |

### Per-League Gap

| League | N | Mean | SD | ±2c% |
|--------|---|------|----|----- |
| Bundesliga | 107 | -0.0066 | 0.0171 | 75.7% |
| EPL | 222 | -0.0015 | 0.0406 | 74.3% |
| La Liga | 159 | -0.0011 | 0.0180 | 76.1% |
| Ligue 1 | 29 | -0.0014 | 0.0215 | 72.4% |
| Serie A | 111 | +0.0016 | 0.0914 | 66.7% |

### Strategy (hold to resolution, 2c fees)

| θ | Signals | Buy | Sell | Events | P&L/c | 95% CI | Top-10 | Verdict |
|---|---------|-----|------|--------|-------|--------|--------|---------|
| 2% | 166 | 103 | 63 | 132 | -0.0127 | [-0.0861, +0.0623] | -511.9% | NOT |
| 4% | 51 | 27 | 24 | 41 | -0.0684 | [-0.1860, +0.0567] | -196.6% | NOT |
| 6% | 20 | 8 | 12 | 13 | -0.1175 | [-0.2575, +0.0363] | 24.7% | NOT |

