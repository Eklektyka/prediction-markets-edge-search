## C2 — By-Date Decay Strategy

**Universe:** 6144 resolved markets with 'by <date>'/'before <date>' in question.
**Strategy:** At T−7d / T−3d before close_at, sell YES via fill_after if price ∈ [3c, 15c]. Hold to resolution.
**Fees:** 2c/contract (Polymarket taker).
**Signals:** 236

**I2:** |P&L/contract| ≤ 1 ✓

### Horizon: T-7d

| Metric | Value |
|--------|-------|
| Signals | 106 |
| Mean fill price | 0.077 |
| YES resolution rate | 9.4% |
| Mean P&L/c (net) | -0.0372 (-3.72c) |
| 95% CI | [-9.12c, +1.68c] |
| Median P&L/c (net) | +4.05c |
| Mean lock days | 20.6 |

### Horizon: T-3d

| Metric | Value |
|--------|-------|
| Signals | 130 |
| Mean fill price | 0.073 |
| YES resolution rate | 9.2% |
| Mean P&L/c (net) | -0.0394 (-3.94c) |
| 95% CI | [-8.85c, +0.96c] |
| Median P&L/c (net) | +3.60c |
| Mean lock days | 23.7 |

### By Category (both horizons)

| Category | N | Mean P&L/c (net) | 95% CI | YES rate | Lock days |
|----------|---|------------------|--------|----------|-----------|
| Crypto | 12 | -3.47c | [-18.84c, +11.89c] | 8.3% | 33.5 |
| Culture | 9 | +7.87c | [+5.35c, +10.38c] | 0.0% | 19.8 |
| Finance | 34 | -3.09c | [-12.48c, +6.30c] | 8.8% | 9.5 |
| Politics | 153 | -5.14c | [-9.88c, -0.40c] | 10.5% | 27.2 |
| Sports | 1 | +2.10c | [+2.10c, +2.10c] | 0.0% | 0.7 |
| Tech | 16 | +5.92c | [+4.26c, +7.57c] | 0.0% | 10.2 |
| Weather | 11 | -12.85c | [-36.06c, +10.35c] | 18.2% | 3.8 |

### Concentration (I8)

**Top-10 event share of total P&L:** -19.9%
**Best month:** 2025-07 (+80.70c total, -8.9% of total)
**Ex-best-month P&L:** -987.60c (negative)

**Worst event:** 36712 (-179.00c, 2 signals)

### Monthly P&L

| Month | N | Gross P&L/c | Net P&L/c | Cumul Net |
|-------|---|-------------|-----------|-----------|
| 2025-07 | 17 | +114.70c | +80.70c | +80.70c |
| 2025-08 | 14 | -174.70c | -202.70c | -122.00c |
| 2025-09 | 23 | +74.20c | +28.20c | -93.80c |
| 2025-10 | 24 | -159.40c | -207.40c | -301.20c |
| 2025-11 | 30 | -79.20c | -139.20c | -440.40c |
| 2025-12 | 17 | +31.20c | -2.80c | -443.20c |
| 2026-01 | 40 | +121.50c | +41.50c | -401.70c |
| 2026-02 | 45 | -174.70c | -264.70c | -666.40c |
| 2026-03 | 26 | -188.50c | -240.50c | -906.90c |

### T1–T5 Tradeability

**Eval period:** 2026-01–2026-03 (3 months)
**T1 (CI>0 on eval):** FAIL — mean -4.177c, CI [-9.630c, +1.275c]
**T2 (≥60% months >0):** FAIL — 3/9 = 33%
**T3 (top-10 <50%, ex-best >0):** FAIL — top-10 share -19.9%, ex-best -987.60c
**T4 (latency):** PASS — fill_after based (uses others' trades)
**T5 (≥$100/mo net):** est. $-22/mo at median size 20.5 contracts

**Verdict: NOT TRADEABLE** (1/5)
