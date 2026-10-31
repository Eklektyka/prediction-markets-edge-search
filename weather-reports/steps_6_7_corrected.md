# Steps 6 & 7 — Corrected (Single-Leg + Unit-Aware)

_Generated: 2026-09-13. Script: `step6_7_redo.py`. Data: single-leg trades from `rebuild_single_leg.py`._

---

## Step 6: Headline Table

L=30s, 2c spread haircut, 1% probability that m=1 fills resolve YES. All USD.

| City | 100% | First-mover | 50% | 25% |
|------|------|-------------|-----|-----|
| **NYC median monthly** | $62 | $45 | $31 | $15 |
| NYC worst month | −$10 | −$10 | −$5 | −$3 |
| NYC best month | $233 | $233 | $117 | $58 |
| **London median monthly** | $129 | $147 | $64 | $32 |
| London worst month | $0 | $88 | $0 | $0 |
| London best month | $1,150 | $1,067 | $575 | $288 |
| **Both median monthly** | $177 | $123 | $89 | $44 |
| Both worst month | $51 | $45 | $25 | $13 |
| Both best month | $1,218 | $1,108 | $609 | $304 |

**DJF (Dec–Feb):** NYC DJF median = $51 (100%), $13 (25%). London DJF median = $88 (100%), $22 (25%). Winter is not distinctly worse — the late-max risk is offset by there being fewer °F bins breached per day (narrower temperature range).

### Sensitivity (first-mover, L=30s)

| Adjustment | NYC 9-mo net | London 9-mo net |
|------------|-------------|-----------------|
| Base | $1,135 | $2,169 |
| −2c spread | $800 (−$335) | $1,953 (−$216) |
| +1% m=1 risk | $968 (−$167) | $2,097 (−$72) |
| Combined | **$633** | **$1,881** |

The 2c spread is a larger hit on NYC (−30%) than London (−10%) because NYC fills are cheaper (mean $0.17 vs $0.18), so a 2c absolute haircut is proportionally larger.

### Monthly detail (L=30s, 100% scenario)

**NYC:** 9 months, 148 trading days, $1,315 total net ($8.9/day). Worst month: Feb 2026 ($149, after $27 in losses from 16 YES-resolved fills). Best month: Oct 2025 ($311, zero losses). London has zero losses across all months (the unit fix eliminated all false signals).

**London:** 7 active months (no fills in Jan or Feb 2026), 109 trading days, $2,273 total net ($20.9/day). November 2025 is the outlier ($1,212, one third of all P&L). Without November, the median monthly net drops to $166.

---

## Step 7: Per-City Busted-Bin Table

Gross ceiling proxy: routine METARs that bust a recently-traded bin, notional YES-sold within 10 min. **No fill, capture, or loss modelling.** Single-leg trades, unit-aware comparison.

| City | Events | Busted-bin | Bust/evt | Mean stale $ | Gross/day $ |
|------|--------|-----------|----------|-------------|-------------|
| New York City | 433 | 978 | 2.26 | $16.7 | $38.0 |
| Atlanta | 115 | 307 | 2.67 | $9.0 | $24.2 |
| Miami | 69 | 212 | 3.07 | $7.8 | $24.0 |
| Chicago | 69 | 238 | 3.45 | $6.7 | $23.0 |
| London | 429 | 317 | 0.74 | $28.3 | $21.0 |
| Seattle | 115 | 236 | 2.05 | $6.2 | $12.7 |
| Dallas | 116 | 247 | 2.13 | $3.7 | $7.8 |
| Lucknow | 25 | 4 | 0.16 | $42.3 | $6.8 |
| Wellington | 68 | 85 | 1.25 | $4.7 | $5.9 |
| Buenos Aires | 114 | 122 | 1.07 | $4.6 | $5.0 |
| Ankara | 67 | 93 | 1.39 | $3.4 | $4.7 |
| Tokyo | 21 | 20 | 0.95 | $4.0 | $3.8 |
| Paris | 44 | 39 | 0.89 | $3.3 | $2.9 |
| Toronto | 114 | 75 | 0.66 | $2.0 | $1.3 |
| Tel Aviv | 20 | 53 | 2.65 | $0.4 | $1.0 |
| Munich | 25 | 35 | 1.40 | $0.5 | $0.7 |
| São Paulo | 44 | 54 | 1.23 | $0.3 | $0.4 |
| Singapore | 18 | 13 | 0.72 | $0.4 | $0.3 |
| Seoul | 115 | 170 | 1.48 | $0.1 | $0.2 |
| Shanghai | 18 | 20 | 1.11 | $0.0 | $0.0 |

### March 2026 all-city estimate

| Metric | Value |
|--------|-------|
| NYC + London daily gross ceiling | $59 |
| All 20 cities daily gross ceiling | $177 |
| **Ratio (all / NYC+London)** | **3.0x** |

The corrected scaling factor is **3.0x** (down from the bugged 8.1x). US cities (Atlanta, Miami, Chicago, Seattle, Dallas) add meaningful incremental gross. International cities with °C bins contribute very little after the unit fix — their stale flow is dominated by cheap bins with near-zero notional.

---

## What does this pay per month at realistic capture, and how much does it scale with cities?

**At 25% capture with spread and loss adjustments, NYC and London combined pay a median of ~$44/month** (worst $13, best $304). At first-mover capture: ~$123/month median. At 100%: ~$177/month. These are small numbers — a good month covers a dinner; the best month (November 2025, driven by London) covers a weekend trip. The strategy is real but the market is too thin to generate meaningful income.

**Scaling with cities adds roughly 3x.** The top additional cities (Atlanta, Miami, Chicago) contribute $20–24/day in gross ceiling each, comparable to London's $21/day. At 25% capture across all 20 cities, the all-city monthly estimate would be ~$44 × 3 ≈ $130/month. At first-mover: ~$370/month. The international cities (Seoul, São Paulo, Singapore, Shanghai) contribute near-zero after the °C/°F fix — their stale flow is dominated by penny-priced bins. The scaling multiplier is driven almost entirely by adding US cities with °F bins and sufficient liquidity.
