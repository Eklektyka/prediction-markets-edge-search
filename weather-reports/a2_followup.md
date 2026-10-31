# A2 Follow-Up

_Generated: 2026-09-13. Script: `weather/code/a2_followup.py`_

---

## 1. A2 Capacity: Notional vs Profit

A2 fills are taker buys of the (predicted) winning bin at price ≤ 0.97. **Notional deployed** = price × contracts (capital locked until resolution). **Profit** = (1 − price) × contracts if the prediction is correct, or −price × contracts if wrong.

Phase 2's "$30/day capacity" was contracts per day. Here are both notional and profit:

### NYC

| Month | Fills | Contracts | Notional deployed | Profit | Notional/day | Profit/day |
|-------|-------|-----------|-------------------|--------|-------------|------------|
| 2025-07 | 1 | 50 | $0.4 | $49.7 | $0.01 | $1.60 |
| 2025-11 | 9 | 160 | $27.3 | $107.9 | $0.91 | $3.60 |
| 2025-12 | 4 | 86 | $47.5 | $31.8 | $1.53 | $1.03 |
| 2026-01 | 6 | 1,101 | $127.5 | −$88.9 | $4.11 | −$2.87 |
| 2026-02 | 5 | 109 | $24.7 | $84.6 | $0.88 | $3.02 |
| 2026-03 | 8 | 84 | $31.1 | $40.3 | $1.00 | $1.30 |
| **Total** | **33** | **1,591** | **$258.6** | **$225.4** | **$1.42** | **$1.24** |

### London

| Month | Fills | Contracts | Notional deployed | Profit | Notional/day | Profit/day |
|-------|-------|-----------|-------------------|--------|-------------|------------|
| 2025-07 | 1 | 51 | $0.1 | $51.4 | $0.00 | $1.66 |
| 2025-10 | 1 | 73 | $0.1 | $72.9 | $0.00 | $2.35 |
| 2025-11 | 6 | 186 | $76.4 | $107.2 | $2.55 | $3.57 |
| 2025-12 | 3 | 271 | $99.7 | $171.1 | $3.22 | $5.52 |
| 2026-01 | 6 | 1,725 | $30.5 | $60.7 | $0.98 | $1.96 |
| 2026-02 | 4 | 208 | $7.0 | $200.7 | $0.25 | $7.17 |
| 2026-03 | 4 | 51 | $0.5 | $50.9 | $0.02 | $1.64 |
| **Total** | **25** | **2,565** | **$214.1** | **$714.9** | **$1.01** | **$3.36** |

**Summary:** The original A2 (first fill only) deploys ~$1–1.50/day in notional capital and earns ~$1.25–3.35/day in profit. Both are tiny. The high P&L-per-dollar ratios from Phase 2 are correct but misleading — they reflect the fact that most fills are at penny prices (near-zero notional), not that large capital is being deployed. January 2026 NYC was the only losing month ($-89 from 6 fills that bet on a stale max).

---

## 2. Late-Max Risk from Observations (2024-01-01 to 2026-09-12)

For every day in the METAR record, I apply the A2 signal rule (routine reports only) and flag whether the daily max (round-half-up) ultimately exceeds the running max at signal time.

### Base rule (signal ≥ 17:00, 3 consecutive unchanged routines)

| City | Days with signal | Late-max days | Rate |
|------|-----------------|---------------|------|
| NYC | 983 | 52 | **5.29%** |
| London | 986 | 53 | **5.38%** |

**Breakeven check (base rule):**

| Buy cap | Breakeven max loss rate | NYC actual | London actual | Verdict |
|---------|----------------------|------------|---------------|---------|
| ≤ 0.95 | 5.0% | 5.29% | 5.38% | **FAIL both** |
| ≤ 0.97 | 3.0% | 5.29% | 5.38% | **FAIL both** |
| ≤ 0.985 | 1.5% | 5.29% | 5.38% | **FAIL both** |

**The base rule does not clear breakeven at any price cap.** The 5.3% late-max rate exceeds all breakeven thresholds. The in-sample profitability from Phase 2 was driven by the fill distribution (most fills at near-zero prices), not by the signal being fundamentally safe.

### Strict rule (signal ≥ 19:00, 4 consecutive unchanged routines)

| City | Days with signal | Late-max days | Rate |
|------|-----------------|---------------|------|
| NYC | 972 | 22 | **2.26%** |
| London | 986 | 21 | **2.13%** |

**Breakeven check (strict rule):**

| Buy cap | Breakeven | NYC actual | London actual | Verdict |
|---------|-----------|------------|---------------|---------|
| ≤ 0.95 | 5.0% | 2.26% | 2.13% | **PASS both** |
| ≤ 0.97 | 3.0% | 2.26% | 2.13% | **PASS both** |
| ≤ 0.985 | 1.5% | 2.26% | 2.13% | **FAIL both** |

The strict rule clears breakeven at ≤ 0.95 and ≤ 0.97 but not at ≤ 0.985.

### By season (base / strict)

| Season | NYC base | NYC strict | London base | London strict |
|--------|----------|------------|-------------|---------------|
| DJF | 10.97% | 5.24% | 10.42% | 5.83% |
| MAM | 3.99% | 2.55% | 1.45% | 0.00% |
| JJA | 3.26% | 0.36% | 5.43% | 0.36% |
| SON | 3.09% | 1.04% | 4.64% | 3.09% |

**Winter (DJF) is the danger season.** Late-max rates of 10–11% (base) and 5–6% (strict) far exceed all breakevens. This is physically expected: winter temperature maxima can occur late in the day from warm-air advection events, and nighttime temperatures can rise above daytime peaks during warm fronts.

The strict rule in MAM/JJA has near-zero late-max rates (0–2.5%). A season-conditional strategy could work, but DJF would need to be excluded entirely.

### By signal hour (base rule)

| Signal hour | NYC | London |
|-------------|-----|--------|
| 17:00 | 4.95% (727 days) | 5.26% (950 days) |
| 18:00 | 5.16% (155 days) | 5.88% (34 days) |
| 19:00 | 7.14% (70 days) | 50.0% (2 days) |
| 20:00+ | sparse, noisy | sparse |

Later signal hours have higher late-max rates — counterintuitively. This is because late signals occur on days when the temperature was still rising late, which are exactly the days most likely to see a late max.

---

## 3. A2-Long

**Extended version:** take ALL taker-buy fills at ≤ 0.97 from signal+30s until the last trade of the event (not just the first fill).

### NYC

| Month | Fills | Contracts | Notional | Profit | Losses | Loss fills |
|-------|-------|-----------|----------|--------|--------|------------|
| 2025-07 | 9 | 640 | $5.6 | $634.6 | $0.0 | 0 |
| 2025-08 | 4 | 281 | $1.2 | $279.9 | $0.0 | 0 |
| 2025-09 | 6 | 194 | $81.4 | −$48.9 | −$57.1 | 4 |
| 2025-10 | 11 | 413 | $3.0 | $409.9 | $0.0 | 0 |
| 2025-11 | 158 | 9,697 | $3,243.7 | $4,437.9 | −$879.6 | 58 |
| 2025-12 | 258 | 14,726 | $4,618.7 | $9,625.3 | −$391.9 | 21 |
| 2026-01 | 361 | 28,298 | $10,346.9 | $10,151.3 | −$1,351.0 | 108 |
| 2026-02 | 197 | 5,337 | $1,072.0 | $4,265.1 | $0.0 | 0 |
| 2026-03 | 819 | 36,759 | $24,879.7 | $1,447.8 | −$5,158.9 | 362 |
| **Total** | **1,823** | **96,344** | **$44,252** | **$31,203** | **−$7,839** |

A2-long increases capacity dramatically: 96K contracts / $44K notional / $31K profit over 9 months, compared to A2's 1.6K contracts / $259 notional / $225 profit. But losses are substantial: $7,839 in losses across 553 loss fills (30% of fills). March 2026 is particularly poor: $24.9K deployed, only $1.4K profit after $5.2K in losses.

### London

| Month | Fills | Contracts | Notional | Profit | Losses | Loss fills |
|-------|-------|-----------|----------|--------|--------|------------|
| 2025-07 | 23 | 1,328 | $7.5 | $1,320.3 | $0.0 | 0 |
| 2025-08 | 7 | 241 | $0.3 | $240.8 | $0.0 | 0 |
| 2025-09 | 9 | 206 | $0.2 | $205.7 | $0.0 | 0 |
| 2025-10 | 52 | 3,645 | $1,171.5 | $1,372.6 | −$673.9 | 8 |
| 2025-11 | 353 | 24,925 | $7,121.6 | $15,719.1 | −$1,405.6 | 46 |
| 2025-12 | 168 | 9,167 | $3,652.8 | $2,094.7 | −$1,613.8 | 68 |
| 2026-01 | 471 | 44,625 | $1,446.2 | $19,368.5 | −$695.0 | 58 |
| 2026-02 | 190 | 9,221 | $3,012.9 | $6,208.2 | $0.0 | 0 |
| 2026-03 | 199 | 9,372 | $74.1 | $9,297.8 | $0.0 | 0 |
| **Total** | **1,472** | **102,729** | **$16,487** | **$55,828** | **−$4,388** |

London A2-long is more profitable ($55.8K on $16.5K notional) with lower losses ($4.4K vs $7.8K for NYC). The loss/gross ratio is 7% (London) vs 20% (NYC).

### Taker concentration

| Metric | NYC | London |
|--------|-----|--------|
| Distinct takers | 651 | 541 |
| Top 1 share (contracts) | 4.9% | 8.6% |
| Top 3 share (contracts) | 13.8% | 16.9% |
| Top 10 share (contracts) | 34.2% | 35.6% |
| Top 1 share (P&L) | 15.2% | −0.0% |
| Top 10 share (P&L) | 41.3% | 25.0% |

The flow is moderately concentrated: 10 addresses capture 34–36% of contracts. London's #1 taker by volume (8,860 contracts, 8.6%) has **negative P&L** (−$9) — they traded heavily on late-max days. NYC's #1 (4,735 contracts) earned $4,729.

**A1 overlap:** The top A2-long takers show modest overlap with the A1 post-lock flow. NYC's #1 A2-long taker has 6,039 A1 contracts (1.2% of A1 total). The familiar bot `0x5f390e...` appears as London's #3 taker with 4,211 A2-long contracts and 17,548 A1 contracts (2.9% of A1). These addresses are doing the A2-long trade already — buying winner bins at cheap prices after the outcome is effectively locked — but they represent a small fraction of the total A1 flow. Most of the $1.3M–2.3M in A1 volume (Phase 2) is done by other addresses.
