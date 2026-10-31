# Phase 0 — Data Verification Report

_Generated: 2026-09-13. All timestamps UTC unless noted._

---

## Task 1: Kalshi Weather Markets in the Becker (Lychee) Dump

**Data source:** `C:\Kalshi_data\lychee\data.tar.zst` (33.5 GiB compressed).
Extracted: 769 market files (7,682,445 rows), 7,214 trade files (~72.1M rows).
Date range: markets 2021-06-30 to 2025-11-23; trades 2021-06-30 to 2025-11-25.
**Script:** `weather/code/task1_kalshi_weather.py`

### 1.1 KXHIGH/KXLOW series found

| Series | City | Station (TWC code) | Tickers | Trades | Contracts | First trade | Last trade |
|--------|------|--------------------|---------|--------|-----------|-------------|------------|
| KXHIGHAUS | Austin | CLIAUS | 2,298 | 562,085 | 39,239,667 | 2024-10-24 | 2025-11-24 |
| KXHIGHCHI | Chicago | CLIMDW (Midway) | 2,291 | 502,487 | 39,136,208 | 2024-10-24 | 2025-11-24 |
| KXHIGHDEN | Denver | CLIDEN | 2,152 | 458,479 | 26,287,823 | 2024-11-19 | 2025-11-25 |
| KXHIGHHOU | Houston | CLIHOU (NWS source) | 373 | 62,777 | 6,414,678 | 2024-11-19 | 2025-01-29 |
| KXHIGHLAX | Los Angeles | CLILAX | 1,842 | 535,753 | 27,367,878 | 2025-01-04 | 2025-11-25 |
| KXHIGHMIA | Miami | CLIMIA | 2,150 | 418,309 | 29,804,045 | 2024-10-24 | 2025-11-25 |
| KXHIGHNY | NYC | CLINYC (Central Park) | 2,289 | 806,295 | 52,219,568 | 2024-10-24 | 2025-11-25 |
| KXHIGHPHIL | Philadelphia | CLIPHL | 2,079 | 294,605 | 17,306,667 | 2024-11-19 | 2025-11-24 |
| **Total** | | | **16,332** | **3,640,790** | **237,776,534** | | |

No KXLOW series found. Two non-weather series (KXHIGHMOVDJT, KXHIGHMOVKH — movie box-office) were excluded (15 markets, 3,430 trades).

### 1.2 Settlement sources

All current KXHIGH markets settle via **The Weather Company** (weather.com/kalshi) using TWC station codes.
Exception: Houston (KXHIGHHOU) settled via **NWS CLI product** (forecast.weather.gov) — this series ended 2025-01-29 and may have switched to TWC for later markets.

**Critical difference from Polymarket:** Kalshi NYC settles on **Central Park (CLINYC)**, Polymarket NYC settles on **LaGuardia (KLGA)**. These are different stations ~8 miles apart.

### 1.3 Overlap with Polymarket sample

Polymarket weather sample: 2025-07-01 to 2026-03-29.
Kalshi weather trades: 2024-10-24 to 2025-11-25.

**Overlap window: 2025-07-01 to 2025-11-25 (~148 days).**

Overlap trades by series:

| Series | Overlap trades | Period |
|--------|---------------|--------|
| KXHIGHAUS | 210,048 | Jul–Nov 2025 |
| KXHIGHCHI | 190,735 | Jul–Nov 2025 |
| KXHIGHDEN | 181,094 | Jul–Nov 2025 |
| KXHIGHLAX | 365,021 | Jul–Nov 2025 |
| KXHIGHMIA | 188,151 | Jul–Nov 2025 |
| KXHIGHNY | 352,772 | Jul–Nov 2025 |
| KXHIGHPHIL | 142,161 | Jul–Nov 2025 |
| **Total** | **1,629,982** | |

Houston has no overlap (series ended Jan 2025). Only US cities available on Kalshi — no London overlap possible.

### 1.4 Kalshi trades by month

| Month | Trades |
|-------|--------|
| 2024-10 | 17,204 |
| 2024-11 | 311,053 |
| 2024-12 | 310,800 |
| 2025-01 | 248,329 |
| 2025-02 | 237,492 |
| 2025-03 | 238,189 |
| 2025-04 | 217,601 |
| 2025-05 | 221,912 |
| 2025-06 | 208,228 |
| 2025-07 | 234,630 |
| 2025-08 | 219,550 |
| 2025-09 | 290,090 |
| 2025-10 | 522,812 |
| 2025-11 | 362,900 |

**Verdict:** There is a substantial Kalshi weather leg — 8 US cities, 3.6M trades, Oct 2024 to Nov 2025. A 148-day overlap window (Jul–Nov 2025) exists for 7 cities with Polymarket. Cross-venue comparison is feasible for US cities in this window but station differences (Central Park vs LaGuardia for NYC) must be accounted for.

---

## Task 2: Lead-Time Availability (NYC and London)

**Script:** `weather/code/task2_lead_times.py`
**Source:** eval_extract trades (2025-07-01 to 2026-03-29) + markets.parquet from Autotrader.

### 2.1 Lead-hours histogram (first trade to local midnight of settlement date)

#### New York City (437 events, 276 with trades)

| Bucket | Full sample | Share | Before Dec 2025 | After Dec 2025 |
|--------|-------------|-------|-----------------|----------------|
| 0–24 h | 47 | 17.2% | 39 (25.5%) | 8 (6.6%) |
| 24–48 h | 191 | 69.7% | 98 (64.1%) | 93 (76.9%) |
| 48–72 h | 18 | 6.6% | 12 (7.8%) | 6 (5.0%) |
| 72–96 h | 18 | 6.6% | 4 (2.6%) | 14 (11.6%) |
| >96 h | 0 | 0.0% | 0 | 0 |

#### London (434 events, 276 with trades)

| Bucket | Full sample | Share | Before Dec 2025 | After Dec 2025 |
|--------|-------------|-------|-----------------|----------------|
| 0–24 h | 76 | 27.7% | 55 (36.2%) | 21 (17.2%) |
| 24–48 h | 161 | 58.8% | 79 (52.0%) | 82 (67.2%) |
| 48–72 h | 19 | 6.9% | 15 (9.9%) | 4 (3.3%) |
| 72–96 h | 18 | 6.6% | 3 (2.0%) | 15 (12.3%) |
| >96 h | 0 | 0.0% | 0 | 0 |

### 2.2 Lead-time cut analysis

Cut = 12:00 UTC on (settlement_date - N days). "Share" = fraction of traded events with at least one trade in any bin before the cut.

| N days | NYC: share with trade | NYC: median bins traded | London: share with trade | London: median bins traded |
|--------|-----------------------|------------------------|--------------------------|---------------------------|
| 1 | 89.5% | 7.0 | 92.8% | 7.0 |
| 2 | 23.9% | 7.0 | 34.8% | 7.0 |
| 3 | 8.7% | 11.0 | 8.0% | 11.0 |
| 4 | 1.8% | 11.0 | 3.6% | 4.0 |
| 5 | 0.0% | — | 0.0% | — |
| 6 | 0.0% | — | 0.0% | — |
| 7 | 0.0% | — | 0.0% | — |

### 2.3 Trade volume per event

| City | Events traded | Median trades/event | IQR | Median trades/bin | IQR |
|------|--------------|---------------------|-----|-------------------|-----|
| NYC | 276 | 1,292 | [740, 3,411] | 160 | [90, 422] |
| London | 276 | 1,451 | [827, 3,859] | 198 | [108, 519] |

**Verdict:** N=1 day (12:00 UTC the day before settlement) is the only usable lead time with near-complete coverage (~90%). N=2 days drops to 24–35%. N>=5 has zero coverage. This means the paper's forecast comparison is limited to day-ahead (and possibly 2-day) horizons. Markets are posted ~24–48 hours before settlement, consistent with Polymarket listing daily events the morning before. Both cities have adequate liquidity (median ~1,300 trades/event).

---

## Task 3: METAR Daily Max vs Resolved Polymarket Bin

**Script:** `weather/code/task3_metar_truth.py`
**METAR source:** Iowa Environmental Mesonet ASOS download service.
URL pattern: `https://mesonet.agron.iastate.edu/cgi-bin/request/asos.py?station={code}&data=tmpf&...`
Date range requested: 2024-01-01 to 2026-09-14. All report types including SPECIs.

### 3.1 New York City — LGA (LaGuardia)

- METAR observations: 27,165 valid tmpf readings across 988 days
- Resolved Polymarket events with METAR coverage: **432**

| Rounding | Matches | Match rate |
|----------|---------|------------|
| Round-half-up | 430 / 432 | **99.5%** |
| Floor | 428 / 432 | 99.1% |

#### Mismatches (round-half-up convention, 2 total)

| Date | Resolved bin | METAR max (°F) | Round | Floor | Max obs (UTC) | Obs | Hours |
|------|-------------|----------------|-------|-------|---------------|-----|-------|
| 2026-02-01 | 24-25°F | 25.0 | 25 | 25 | 2026-02-01 20:51 | 27 | 24 |
| 2026-02-02 | 35-36°F | 35.0 | 35 | 35 | 2026-02-02 19:51 | 24 | 24 |

Both mismatches are boundary cases: METAR max = exact lower bound of a 2°F bin, and the market resolved to the bin below. This suggests WU may report a value 1°F lower on these days (perhaps from a slightly different observation window or rounding of sub-degree precision in the opposite direction). Neither mismatch is a timezone issue (UTC window and ±1h shift don't resolve them). Both days have full 24-hour observation coverage.

### 3.2 London — EGLC (London City Airport)

- METAR observations: 47,332 valid tmpf readings across 987 days
- Resolved Polymarket events with METAR coverage: **428**
- Note: London bins use °F through ~Dec 10 2025, then switch to °C.

| Rounding | Matches | Match rate |
|----------|---------|------------|
| Round-half-up | 428 / 428 | **100.0%** |
| Floor | 354 / 428 | 82.7% |

Zero mismatches under round-half-up. The floor convention drops to 82.7% — expected because IEM reports in fractional °F (converted from °C METAR), and rounding vs flooring diverges at .5 boundaries. **Round-half-up is the correct convention for matching WU's settlement.**

### 3.3 Timezone edge analysis

No mismatches were resolved by switching to a UTC day window or ±1h shift for either city. No days had fewer than 20 hourly reports. The timezone definition (local midnight-to-midnight) is correct.

**Verdict:** METAR daily max is an excellent ground truth. NYC achieves 99.5% match (430/432), London 100.0% (428/428). The two NYC mismatches are ±1°F boundary cases on days with full observation coverage, likely due to sub-degree rounding differences between IEM's METAR archive and WU's reported value. Round-half-up is the correct rounding convention. Match rate exceeds the 98% threshold for both cities.

---

## Unable to Verify

1. **Kalshi settlement station identity from the Lychee dump itself.** The market `title` field in the dump contains city names but not station codes. Station codes (CLINYC, CLIMDW, etc.) were obtained from the live Kalshi API for current markets. Historical markets (2024–2025) may have used different stations or NWS as the settlement source; the dump does not contain a `rules_primary` field to confirm.

2. **KXLOW series.** No KXLOW tickers were found in the Lychee dump. This series may not have existed during the dump's coverage period (through Nov 2025), or may use a different ticker prefix.

3. **London METAR precision.** IEM reports EGLC temperatures in °F with one decimal place (e.g., 48.2°F = 9.0°C). The original METAR for EGLC is in °C (whole degrees). Our °C bin matching converts IEM's °F back to °C via rounding, which introduces a double-conversion. A direct °C METAR source would eliminate this. For phase 0 the 100% match rate confirms this is not currently causing errors, but it could matter at scale.

4. **Events without trades.** ~37% of NYC/London events (161/437 and 158/434) had zero trades in the eval_extract sample. These are listed in markets.parquet but either never traded or traded outside the sample window. They are excluded from all lead-time and match-rate calculations.
