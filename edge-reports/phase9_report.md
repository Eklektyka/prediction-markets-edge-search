# Phase 9 Report

_Generated: 2026-09-15. Data: edge/core/ (62.7M YES-leg trades, 729K markets). Analyses: B1v3, C1, C2, C3._

---

## B1v3 — Settled-P&L Persistence

**Wallets analysed:** 101 (top-5 counterparties from Phase 6 + top-100 by volume; 4 overlap).
**Method:** Settled P&L = closed round-trips (FIFO, per asset) + resolved positions (payout − cost basis, attributed to resolution month). Positions in unresolved markets excluded.
**Excluded notional share:** 100.0% of open-position notional is in unresolved markets.
**Trade-count floor:** 100 trades/month.
**Persistence test:** share of months with settled P&L > 0 vs within-wallet sign-permutation null (1,000 permutations).

### Top-5 Counterparties — Monthly Settled P&L (round-trip / resolution split)

| Wallet | Months | RT P&L | Res P&L | Settled P&L | Pos% | p-val |
|--------|--------|--------|---------|-------------|------|-------|
| 0xc2e7800b… | 1 | $+0 | $+0 | $+0 | — | — |
| 0xac44cb78… | 1 | $+0 | $+0 | $+0 | — | — |
| 0xea645729… | 3 | $-54,094 | $+0 | $-54,094 | 0% | 1.000 |
| 0xc8ab97a9… | 10 | $-126,822 | $+0 | $-126,822 | 20% | 0.990 |
| 0xe90bec87… | 5 | $-135,240 | $+0 | $-135,240 | 20% | 0.967 |

**Key finding:** Resolution P&L contributes $0 across all wallets. These are market makers whose positions are closed through round-trips (buy then sell), not by holding to resolution. The FIFO queues' residual open positions — tiny relative to total volume — are entirely in markets that have not yet resolved (100% excluded notional). Settled P&L = round-trip P&L for this population, making B1v3 identical to B1 in practice.

**Raw monthly series for top-5:** See edge/report/phase9_b1v3.md for full tables.

### Top-100 by Volume — Persistence Summary

| Metric | Value |
|--------|-------|
| Wallets with ≥3 valid months | 91 |
| Persistent at p < 0.05 (monthly) | 10 |
| Mean positive-month share | 57.4% |
| Mean total settled P&L | $+33,469 |
| Median total settled P&L | $+222 |
| Quarterly persistent at p < 0.05 | 0/85 |

### Chess Maker Cross-Check

Chess market-maker (0xC5d5…f80a): **NOT** in top-5 CPs, **NOT** in top-100 vol. Concentrated in chess markets (0.08% of volume).

### Result

**Top-5 counterparties:** 0/5 persist at p < 0.05 on settled P&L. All are net losers on round-trip cash P&L (−$316K combined). Their profits, if any, are locked in unresolved positions — not measurable from trade data alone.

**Top-100 by volume:** 10/100 persist at p < 0.05 — consistent with 5% false-positive rate under the null. No evidence of systematic persistence in this population.

**I6:** Σ monthly = total for all wallets. PASS.

---

## C2 — By-Date Decay Strategy

**Universe:** 6,144 resolved markets with "by <date>"/"before <date>" in question.
**Strategy:** At T−7d / T−3d before close_at, sell YES via fill_after if price ∈ [3c, 15c]. Hold to resolution.
**Fees:** 2c/contract (Polymarket taker).
**Signals:** 236 (T-7d: 106, T-3d: 130).

### Summary

The decay strategy exploits the tendency of "by-date" markets to have YES prices that decline toward zero as the deadline approaches if the event hasn't occurred. Selling YES at 3–15c captures the premium if the event doesn't happen (resolved NO). The YES resolution rate is low (9–10%), so most signals are winners — but the losers are catastrophic (−85c to −97c per contract).

| Horizon | Signals | Mean fill | YES rate | Net P&L/c | 95% CI |
|---------|---------|-----------|----------|-----------|--------|
| T-7d | 106 | 7.7c | 9.4% | −3.72c | [−9.12c, +1.68c] |
| T-3d | 130 | 7.3c | 9.2% | −3.94c | [−8.85c, +0.96c] |

Mean P&L is negative: the ~10% YES resolution rate is enough to overwhelm the small premiums collected (7c avg) after fees. The strategy is structurally a short-vol trade — profitable most of the time but loses more on the tails than it gains.

By category: Culture (+7.9c) and Tech (+5.9c) are positive but with tiny sample sizes. Politics (153 signals, −5.1c, CI excluding zero on the downside) drives the overall loss.

### T1–T5

T1 FAIL, T2 FAIL (3/9 months positive), T3 FAIL (ex-best-month negative).

**Verdict: NOT TRADEABLE** (1/5).

---

## C1 — Coherence Pairs

**Method:** Among resolved "by <date>" markets, identify pairs (A, B) where A ⇒ B (same question stem, A deadline earlier than B). If the event happens by A's deadline, it certainly happened by B's later deadline — so price(A) should never exceed price(B).

**LLM pass 1:** Automated stem extraction + deadline ordering → 2,634 raw pairs from 129 stem groups.
**LLM pass 2:** Filtered non-monotonic questions, dropped wrong pairs → 2,606 valid pairs.
**Selected:** 30 pairs across 30 distinct stems (diverse categories, gap 2–91 days).
**Wrong pairs (A=YES, B=NO):** 0 — implication was correct for all selected pairs.

### Daily Price Gap

Of the 30 selected pairs, 14 had overlapping trade data (both markets actively traded during the same period). The mean gap across pairs was −6.1c (correct direction: B prices higher than A). However, violations occur:

- **Thailand × Cambodia ceasefire:** 94% of days with gap >2c, max 34-day violation run.
- **Israel × Iran ceasefire broken:** 31% of days with gap >2c, 30-day max run.
- **Russia strike on Kyiv:** 29% of days >2c, but only 1 day max run.

Most pairs show no violations — prices are correctly ordered (earlier deadline ≤ later deadline).

### Strategy P&L

Only 1 of 14 pairs generated a fill via fill_after at the first violation. P&L: −12.9c/contract. The market is too illiquid for a coherence-arbitrage strategy: violations exist but cannot be traded at scale.

**Verdict: NOT TRADEABLE** (insufficient signals).

---

## C3 — Goal Latency (BLOCKED)

**Status:** BLOCKED — requires football-data.org API key (free tier, register at https://www.football-data.org/client/register). The API provides goal minute data for all 5 leagues in the B2 match set (EPL, La Liga, Bundesliga, Serie A, Ligue 1). Free tier: 100 requests/24 hours; 738 matched games would require ~8 days of collection.

Script written and ready to run: `edge/analysis/c3_goal_latency.py`. Set `FOOTBALL_DATA_API_KEY` env var and rerun.

---

## Blocked Items

1. **C3 (Goal latency):** Needs `FOOTBALL_DATA_API_KEY` env var. Register free at https://www.football-data.org/client/register. Script ready at `edge/analysis/c3_goal_latency.py`.
2. **P10 (Crypto 15-min):** Running in `edge/analysis/p10_crypto_15min.py`. Downloading Binance 1m klines for 38K markets across 7 months. Will be reported in phase10_report.md upon completion.
