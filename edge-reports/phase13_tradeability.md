# Phase 13 — Tradeability Close-Out

_Generated: 2026-09-15._

## Summary

| Question | Verdict | $/month at capital | What would flip it |
|----------|---------|--------------------|--------------------|
| Q1 Crypto 15-min | NOT TRADEABLE | $0 (1c) | No first-mover fills |
| Q2 Politics maker | BLOCKED | — | Set POLYGON_RPC_URL |
| Q3 Longshot premium | MARGINAL | $16/mo on $5K | Scale + tail management |

---

## Q1 — Crypto 15-Min Up/Down: Is the 3c/Contract Real?

### Q1.1 Coverage

**2270/38059** markets (6.0%) have trades in core. Audited 20 unmatched markets: all have **0 trades** in trades.parquet. Not a join bug — most 15-min windows simply go untraded. The 2,270 assets are the full tradeable universe. No fix needed.

### Q1.2 Fees

**Fee schedule:** Polymarket crypto taker fee = `C × 0.07 × p × (1−p)` ([source](https://www.theblock.co/post/384461/polymarket-adds-taker-fees-to-15-minute-crypto-markets-to-fund-liquidity-rebates)). Peaks at 1.75c at p=50c, drops to 0.63c at p=10c/90c. Maker fee = 0. Previous analysis used flat 2c — recomputed with correct curve below.

### Q1.3 Settlement Source

**Settlement:** 15-min markets settle on **Chainlink 60-second TWAP** at window close, with open = Chainlink snapshot at window start ([source](https://dev.to/bluewhale-quant-lab/how-polymarket-decides-up-or-down-reconstructing-the-price-to-beat-129d)). Fair value model used **Binance 1m klines**. Chainlink aggregates across exchanges (including Binance) with a median filter, so Binance close ≈ Chainlink to within a few bps for BTC/ETH. The 60s TWAP at close slightly smooths the settlement vs a spot print, which could reduce edge for last-second trades (D bucket). **Impact:** Fair value model is approximate but not systematically biased.

### Q1.4 First-Mover-Only P&L

Fills where no other trade on that asset on the same side between signal and signal+latency.

**θ = 1c:**

| Latency | Signals | Fills (FM) | Mean P&L/c | 95% CI | Months pos |
|---------|---------|------------|------------|--------|------------|
| 2s | 24389 | 0 | — | — | — |
| 10s | 281 | 0 | — | — | — |
| 30s | 423 | 0 | — | — | — |

**θ = 2c:**

| Latency | Signals | Fills (FM) | Mean P&L/c | 95% CI | Months pos |
|---------|---------|------------|------------|--------|------------|
| 2s | 22512 | 0 | — | — | — |
| 10s | 263 | 0 | — | — | — |
| 30s | 386 | 0 | — | — | — |

**T1–T5 on first-mover set (θ=1c, 10s latency):**

Insufficient first-mover signals for T1–T5.
**Verdict: NOT TRADEABLE**

### Q1.5 Staleness Split

| Staleness | N | Mean P&L/c | 95% CI |
|-----------|---|------------|--------|
| fresh_<10s | 43928 | +3.74c | [+3.35c, +4.13c] |
| mid_10-60s | 3739 | +2.23c | [+1.00c, +3.47c] |
| stale_>60s | 587 | +1.73c | [-1.24c, +4.71c] |

Edge present in both fresh and stale buckets — not purely a staleness artifact.

### Q1.6 Capacity

### Side Result: ETH Streak Bias (k=3)

From P10 Step 5A: after 3 consecutive up windows, ETH YES price at open = 0.442 but realised up rate = 0.300. Bias = +0.142 (overpricing of continuation). N=60 — suggestive but thin sample. Would need >200 observations to confirm.

---

## Q2 — Politics Maker: What Does It Actually Pay?

**BLOCKED:** `POLYGON_RPC_URL` not set. Cannot access on-chain data for rewards, off-book settlement, or the full income statement. Script ready at `edge/analysis/s2_onchain.py`.

From S1 (trade data only): top-5 makers earn +0.74c/contract spread income. But the full picture (rewards + off-book) requires on-chain data.

---

## Q3 — Binary Longshot Premium: Capital-Bound but Real?

**Universe:** Polymarket sell-YES fills at ≤10c with 30–180 days to resolution.
**Signals:** 2,630,200
**Fees:** Category-specific: `C × rate × p × (1−p)`. Politics 4%, Sports 5%, Crypto 7%.

### By Category

| Category | N | Mean P&L/c | 95% CI | YES rate | Mean days | Ann. return |
|----------|---|------------|--------|----------|-----------|-------------|
| Crypto | 69703 | +2.40c | [+2.35c, +2.45c] | 0.4% | 63 | 18% |
| Culture | 425418 | +0.13c | [+0.09c, +0.16c] | 1.3% | 67 | 1% |
| Finance | 183255 | +0.65c | [+0.60c, +0.70c] | 1.3% | 64 | 3% |
| Politics | 604743 | +0.64c | [+0.62c, +0.67c] | 1.1% | 65 | 4% |
| Sports | 1249744 | +0.44c | [+0.42c, +0.46c] | 0.9% | 104 | 4% |
| Tech | 95271 | +0.51c | [+0.46c, +0.57c] | 0.8% | 54 | 3% |
| Weather | 2066 | -0.24c | [-0.86c, +0.39c] | 2.5% | 76 | -9% |

### Overall

**Mean P&L/contract (net):** +0.504c
**95% CI:** [+0.492c, +0.517c]
**Mean capital locked:** 0.984
**Mean days to resolution:** 83
**Mean annualised return on locked capital:** 3.8%
**YES (loss) rate:** 27078/2630200 = 1.0%

### Monthly P&L

| Month | N | Net P&L (c) | Cumul |
|-------|---|-------------|-------|
| 2025-07 | 225082 | +296500.9c | +296500.9c |
| 2025-08 | 189870 | +176235.9c | +472736.8c |
| 2025-09 | 224493 | +26905.5c | +499642.2c |
| 2025-10 | 422663 | +102527.6c | +602169.8c |
| 2025-11 | 486253 | +131870.0c | +734039.9c |
| 2025-12 | 276017 | +22885.7c | +756925.5c |
| 2026-01 | 294478 | +222610.1c | +979535.6c |
| 2026-02 | 431089 | +292165.4c | +1271701.0c |
| 2026-03 | 80255 | +54896.0c | +1326597.0c |

### Worst-Quarter Drawdown

**Worst quarter:** 2025-Q4 (+257283.3c)

### Loss Correlation (30-day windows)

**Mean losses within ±30 days of each loss:** 55.7
**Total losses:** 27078

### T1–T5

**T1 (CI>0):** PASS — [+0.492c, +0.517c]
**T2 (≥60% months >0):** PASS — 9/9
**T3 (concentration):** PASS — top-10 45%, ex-best +1030096c
**T4 (latency):** PASS — fill_after based
**T5 (≥$100/mo on $5K):** $16/mo

**$/month on $5,000 at realised annualised rate:** $16

**Verdict: MARGINAL**
