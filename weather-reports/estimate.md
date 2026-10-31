# Data-Only Tradeability Estimate

_Generated: 2026-09-13. Script: `estimate.py`. Single-leg, unit-corrected, tail-bin-fixed data._
_Cities: NYC (LGA), London (EGLC), Atlanta (ATL), Miami (MIA), Chicago (ORD)._

---

## 1. Part A — NYC Resolved-YES Audit

Prior runs showed 16 NYC B3 fills whose sold bin resolved YES. The audit classified them:
- **Impossibility-check bug: 10.** All 10 were on upper-tail bins ("X°F or higher") where `hi = None`. The bin-midpoint comparison (`mp < nm_native - 1`) passed because the midpoint of "60°F or higher" is 61, which is less than nm_native − 1 — but the bin includes all temperatures ≥60°F and is never impossible regardless of the running max. These were false signals.
- **Late-max (genuine loss): 6.** The remaining fills were on days where the final daily max did land in the sold bin.

**Fix applied:** Bins with no upper bound (`hi = None`) are now excluded from the busted-bin set. After the fix, NYC has **0 resolved-YES B3 fills** — all genuine late-max losses were on tail bins that should never have been signalled. London was already at 0 (from the °C/°F fix). The corrected B3 totals used below incorporate this fix.

---

## 2. Part C — Latency Sweep

First-mover scenario, 2c spread, 1% m=1 risk. All USD.

| L (s) | NYC med | NYC worst | NYC best | NYC 9-mo | NYC # | LON med | LON worst | LON best | LON 9-mo | LON # | Both med | Both 9-mo |
|-------|---------|-----------|----------|----------|-------|---------|-----------|----------|----------|-------|----------|-----------|
| 30 | −$57 | −$117 | $132 | −$134 | 460 | −$6 | −$111 | $959 | $1,118 | 430 | −$22 | $984 |
| 60 | −$53 | −$121 | $118 | −$188 | 390 | −$16 | −$99 | $975 | $1,163 | 352 | −$3 | $974 |
| 90 | −$31 | −$110 | $5 | −$404 | 320 | −$14 | −$154 | $107 | −$77 | 217 | −$68 | −$481 |
| 120 | −$30 | −$80 | $1 | −$358 | 251 | −$10 | −$112 | $32 | −$125 | 159 | −$71 | −$483 |
| 180 | −$26 | −$67 | −$2 | −$280 | 119 | −$12 | −$89 | $1 | −$241 | 98 | −$56 | −$520 |
| 300 | −$2 | −$42 | $0 | −$90 | 20 | $0 | $0 | $4 | $3 | 5 | −$1 | −$86 |
| **L_fast** | **$18** | **−$78** | **$132** | **$125** | **507** | **−$4** | **−$121** | **$953** | **$1,133** | **497** | **−$18** | **$1,258** |
| L_typ | −$31 | −$110 | $5 | −$403 | 327 | −$16 | −$99 | $966 | $1,164 | 344 | −$23 | $761 |
| **floor** | **−$2** | **−$42** | **$0** | **−$90** | **20** | **$0** | **$0** | **$4** | **$3** | **5** | **−$1** | **−$86** |

**L_fast** = 10th percentile of t_first_event: NYC 5s, London 6s. **L_typ** = median: NYC 88s, London 62s.

**5-city total at L_fast:**

| City | L_fast (s) | Fills | 9-mo P&L |
|------|-----------|-------|----------|
| NYC | 5 | 507 | $125 |
| London | 6 | 497 | $1,133 |
| Atlanta | 3 | 146 | $33 |
| Miami | 5 | 92 | $8 |
| Chicago | 9 | 109 | −$71 |
| **All 5** | | **1,351** | **$1,228** |

Median monthly (5 cities): −$18. Worst: −$127. Best: $1,008.

---

## 3. Part B — Reaction-Time Envelope

Seconds from METAR valid time to first trade. Busted-bin events only (routine or SPECI METAR that raises running max past a bin's upper bound, where that bin traded in the prior 60 min).

| City | Busted events | No trade 10 min | t_first_sell p10 | p25 | p50 | p75 | p90 |
|------|--------------|-----------------|------------------|-----|-----|-----|-----|
| NYC | 1,063 | 47% | 21s | 60s | 115s | 173s | 263s |
| London | 943 | 42% | 14s | 52s | 83s | 142s | 257s |
| Atlanta | 354 | 57% | 7s | 23s | 73s | 137s | 162s |
| Miami | 232 | 55% | 5s | 35s | 77s | 142s | 171s |
| Chicago | 263 | 56% | 29s | 39s | 77s | 134s | 145s |

| City | t_first_event p5 | p10 (L_fast) | p50 (L_typ) | p90 |
|------|-----------------|-------------|-------------|-----|
| NYC | 3s | **5s** | **88s** | 292s |
| London | 3s | **6s** | **62s** | 249s |
| Atlanta | 1s | **3s** | **63s** | 604s |
| Miami | 1s | **5s** | **63s** | 696s |
| Chicago | 6s | **9s** | **59s** | 359s |

47–57% of busted-bin events see **no trade at all** in the busted bin within 10 minutes. This is the "no counterparty" problem — even when a bin is provably impossible, often nobody has a resting order there to hit.

![CDF of t_first_sell](../figs/cdf_t_first_sell.png)

---

## 4. Part D — Concentration and Dependence

At L_fast, first-mover, 2c spread, 1% m=1 risk.

**NYC** (L_fast=5s, $125 total over 9 months):
- Top 10 busted-bin events contribute $561 — **449% of total P&L.** The strategy loses money on most fills and recovers it on a handful of expensive ones.
- By fill price: <$0.05 fills (411 of 507) generate **−$807**; fills >$0.50 (10 of 507) generate **+$407**. The penny bins are net-destructive after the 2c spread haircut.
- NYC's P&L is near zero and entirely dependent on rare high-price fills.

**London** (L_fast=6s, $1,133 total):
- Top 10 events = $1,515 (134% of total). Excluding November 2025 alone: $180 (16% of total).
- By fill price: <$0.05 fills (381 of 497) generate −$897; fills >$0.50 (13 of 497) generate +$1,338.
- London's P&L is overwhelmingly driven by November 2025 and a dozen expensive fills. The median month is −$4.

---

## 5. Summary

The fastest observed market participant reacts to a new-max METAR within **3–6 seconds** (p10 of t_first_event across all five cities). The floor — fills where no one sells the busted bin for five full minutes — pays **−$86 over nine months** across both cities (25 fills total); the stale liquidity that survives five minutes is nearly always in penny-priced bins that lose money after the 2c spread. The L_fast first-mover figure across all five cities is **$1,228 over nine months** (median monthly −$18), entirely carried by one month of London trading in November 2025; excluding that month, all five cities combined net $220 over nine months.
