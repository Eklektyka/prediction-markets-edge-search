# Trade-Leg Integrity Check

_Generated: 2026-09-13. Scripts: `leg_check.py`, `rebuild_single_leg.py`._

---

## 1. How eval_extract built the weather trades

**Source:** HuggingFace dataset `vgregoire/polymarket-users`, streamed one daily partition at a time via `hf_hub_download()`. Code path: `Autotrader/phase2_drift.py → step1_extract()`.

**Complement-leg fix:** **NOT APPLIED.** The eval_extract pipeline ingests the HF data as-is with no YES/NO token filtering. The Phase 0 HF inventory QA (`Autotrader/qa/phase0_hf_inventory.md`) noted that 279/596 multi-prediction_id trade groups showed complementary prices (~1.0 sum), but this was logged as a diagnostic, not acted upon.

**The HF dataset records BOTH the YES-token and NO-token legs of each trade.** Each Polymarket market has exactly 2 `prediction_id`s (YES and NO). When filtering by `market_id`, both legs appear. Empirically:

- Rows / unique trade_id = **1.39** (should be 1.0 for single-leg)
- For dual-prediction trades, price sums average **1.000** — confirmed complement legs
- 27% of rows are NO-leg trades (dropped after fix)

**Weather is NOT treated differently** — the same wallet-filter extraction applies to all categories.

**The `complement_fix.py` in Prediction_Markets_Public is for UFC Lychee data only** and was never applied to eval_extract or weather trades.

## 2. Empirical S check (10 random events per city)

Before fix (mirrored data):

| Event | City | Settle | Median S (30-min fresh) |
|-------|------|--------|------------------------|
| 60944 | NYC | 2025-10-19 | 1.757 |
| 81519 | NYC | 2025-11-17 | 1.761 |
| 50144 | NYC | 2025-09-28 | 1.003 |
| 172006 | NYC | 2026-01-20 | 1.018 |
| 55597 | LON | 2025-10-09 | 2.021 |
| 220998 | LON | 2026-02-23 | 1.729 |
| 85452 | LON | 2025-11-21 | 1.244 |
| 53544 | LON | 2025-10-07 | 1.335 |
| 216584 | LON | 2026-02-21 | 1.084 |

After fix (single-leg):

| City | Rows (all bins fresh) | Median S | S in [0.97, 1.03] |
|------|----------------------|----------|-------------------|
| NYC | 2,258 | **1.003** | **53.1%** |
| London | 1,586 | **1.002** | **58.1%** |

S ≈ 1.0 after the fix, confirming single-leg data is correct.

## 3. B3 fills where the sold bin resolved YES

| Period | City | B3 fills | Resolved YES | Rate |
|--------|------|----------|-------------|------|
| Before fix (mirrored) | NYC | 411 | 15 | 3.6% |
| Before fix (mirrored) | London | 1,353 | 320 | **23.7%** |
| After leg fix only | NYC | 212 | 17 | 8.0% |
| After leg fix only | London | 995 | 281 | **28.2%** |
| After leg + unit fix | NYC | 212 | 17 | 8.0% |
| After leg + unit fix | London | 135 | **0** | **0.0%** |

London's 28% resolved-YES rate was caused by a **°C/°F unit mismatch** in the "impossible bin" check. The B3 signal computed `bin_midpoint < running_max - 1`, where bin midpoints were in °C (e.g., 14) but running_max was in °F (e.g., 46.4). Since 14 < 45.4, bins far ABOVE the running max (in °C) were falsely flagged as "below" it. After converting to the bin's native unit before comparison, London's resolved-YES rate drops to 0%.

NYC's 8.0% rate (17 fills) is genuine: these are late-max days where the temperature rose after the B3 signal. This is consistent with the 5.3% base late-max rate found in the A2 follow-up.

**Fill-price distribution for NYC's 17 YES-resolved fills:**
p5=$0.16, p25=$0.34, p50=$0.54, p75=$1.00, p95=$1.00.

## 4. Corrected Strategy D and B3

### Strategy D — single-leg (corrected)

Phase 2 reported median S = 1.78–2.05 and concluded "structural overpricing from negRisk design." **This was wrong.** The S >> 1 was caused by summing both YES and NO prices. After the single-leg fix, S ≈ 1.00 and the prices do sum to approximately $1 as expected. Complete-set arbitrage remains inapplicable (no systematic S < 0.97 deviations), but the reason is the market is well-arbitraged, not a structural impossibility.

### B3 — single-leg + unit fix (corrected)

| City | Lat | Fills | YES-res | Contracts | Total P&L | Mean P&L/c | Mean fill$ | CI |
|------|-----|-------|---------|-----------|-----------|------------|------------|------|
| NYC | 5s | 212 | 17 | 25,810 | $1,511 | $0.059 | $0.186 | [0.079, 0.135] |
| **NYC** | **30s** | **208** | **16** | **25,630** | **$1,315** | **$0.051** | **$0.174** | **[0.071, 0.125]** |
| LON | 5s | 135 | 0 | 12,387 | $2,257 | $0.182 | $0.181 | [0.144, 0.220] |
| **LON** | **30s** | **135** | **0** | **12,484** | **$2,273** | **$0.182** | **$0.176** | **[0.141, 0.215]** |

**Comparison to Phase 2 (mirrored):**

| City | Lat=30s | Phase 2 P&L | Corrected P&L | Reduction |
|------|---------|-------------|---------------|-----------|
| NYC | 30s | $28,075 | **$1,315** | **−95%** |
| London | 30s | $35,801 | **$2,273** | **−94%** |

**The edge is real but 20x smaller than reported.** NYC earns $0.05/contract (not $0.43) and London $0.18/contract (not $0.45). CIs still exclude zero. The corrected mean fill price is ~$0.18 (not $0.40–0.52) — most fills are on cheap bins near the tail.

## 5. Holdout

**Cannot be completed via the eval_extract code path.** The HuggingFace dataset `vgregoire/polymarket-users` contains trades only through March 2026 (year=2026/month=01, month=02, month=03). No April+ data is available. The `phase2_drift.py` step1_extract function would fail with a FileNotFoundError for any date after 2026-03-31.

The Polymarket public data-API (`data-api.polymarket.com`) returns trades with incorrect timestamps (all mapped to the query date, not the actual trade date), as reported in Phase 3. No holdout verification is possible without either (a) an update to the HuggingFace dataset, or (b) authenticated CLOB API access, or (c) on-chain trade scraping with block-timestamp resolution.

---

## Corrected Steps 6 and 7

All prior income estimates (Step 6 headline table, Step 7 scaling) were computed on mirrored data with the °C/°F bug. Below are the corrected numbers.

### Step 6 corrected: B3 income estimator (single-leg + unit fix)

Corrected fill counts: NYC 208 fills (was 411), London 135 fills (was 1,353).
Corrected mean fill price: ~$0.18 (was $0.40–0.52).
Corrected total P&L over 9 months: NYC $1,315, London $2,273.

| City | 100% monthly (9 mo avg) | 25% monthly | First-mover monthly |
|------|------------------------|-------------|---------------------|
| NYC | ~$146 | ~$37 | ~$146 |
| London | ~$253 | ~$63 | ~$253 |
| **Both** | **~$399** | **~$100** | **~$399** |

At 25% capture, B3 pays roughly **$100/month** on NYC+London combined, not $930.

### Step 7 corrected: scaling

The Step 7 stale-flow proxy used the same °C/°F buggy comparison for non-US cities. International cities with °C bins (Seoul, Ankara, Wellington, São Paulo, etc.) had grossly inflated busted-bin counts because °C midpoints were compared to °F running maxima. The 8.1x scaling factor is unreliable.

A corrected estimate requires rerunning Step 7 with the unit-aware comparison, but even at the ceiling: if the true scaling factor is, say, 3–4x instead of 8x (US cities scale correctly; international cities are smaller after the fix), the all-city monthly estimate at 25% capture would be ~$300–400/month.

### Bottom line

B3 is real but small. At realistic capture on two cities it pays roughly **$100/month** (25% capture) to **$400/month** (100% capture). Scaling to 20 cities might triple this, but the dominant edge is in cheap bins (mean fill $0.18) and the per-fill profit is 5–18 cents. This is a micro-strategy, not a capital deployment vehicle.
