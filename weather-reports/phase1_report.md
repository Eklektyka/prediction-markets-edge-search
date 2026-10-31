# Phase 1 — Panel Build and Descriptives

_Generated: 2026-09-13. All timestamps UTC unless noted. Conventions in conventions.md._

---

## 1. Trading Window Verification (Task 0)

**Finding: markets trade well past the 12:00 UTC expiration metadata field.** The expiration_time in markets.parquet is NOT the last-trade time.

### Last trade local-hour quantiles

| Percentile | NYC (America/New_York) | London (Europe/London) |
|-----------|------------------------|------------------------|
| p5 | 01:49 | 00:45 |
| p25 | 03:09 | 02:09 |
| p50 | 03:42 | 03:00 |
| p75 | 04:07 | 03:37 |
| p95 | 19:13 | 22:45 |

Note: these local hours are on the day **after** settlement_date (the last trade is typically at 03:00–04:00 the following morning). Expressed as hours from local midnight of settlement day:

| Percentile | NYC | London |
|-----------|-----|--------|
| p5 | +14.2h | +20.7h |
| p25 | +26.2h | +25.0h |
| p50 | +27.4h | +26.5h |
| p75 | +27.9h | +27.3h |
| p95 | +28.3h | +27.8h |

### Intraday volume share on settlement day

| Metric | NYC | London |
|--------|-----|--------|
| Events with any trade after local 14:00 | 260/274 (94.9%) | 270/274 (98.5%) |
| Volume share after local 12:00 | 43.1% | 46.2% |
| Volume share after local 14:00 | 32.8% | 31.2% |

### Analysis window definition

- **Start:** 12:00 UTC on settlement_date − 1
- **End:** min(last_trade_utc, local 23:59 on settlement_date)

This captures the main trading period. The 5% of volume that trades after local midnight (next day) is excluded.

---

## 2. Panel Coverage

**Script:** `weather/code/task1_market_panel.py`
**No CLOB order-book snapshots found** for Jul 2025–Mar 2026 in the Autotrader data directory. Only book_2026-08-13.jsonl exists (outside range). Panel is built from trades only.

| Metric | NYC | London |
|--------|-----|--------|
| Resolved events in panel | 432 | 430 |
| Events with trades in window | 274 | 274 |
| Panel grid rows (15-min) | 42,643 | 38,195 |
| Grid rows with n_bins_traded ≥ 5 | 35,143 (82.4%) | — |
| Trades in analysis window | 436,033 | 487,835 |

### Median staleness (minutes since last trade) by hour-of-window

NYC: staleness is ~45 min during the overnight period (hours −16 to −1 from local midnight), drops to ~25 min during the settlement day (hours +4 to +12), and increases rapidly after local ~15:00 (+15h) as trading thins.

London: similar pattern — ~35 min overnight, drops to ~14 min at peak (hours +8 to +12), then rises after local 14:00.

---

## 3. Error Distribution Tables (Task 3)

**Pre-sample fit period: 2024-01-01 to 2025-06-30.** Residual = METAR − forecast (°F).

### KLGA (NYC) — day1 vintage

| Model | n | Bias | SD | Skew | Kurt | p5 | p25 | p50 | p75 | p95 |
|-------|---|------|----|----|------|----|----|----|----|-----|
| GFS | 528 | +0.87 | 3.27 | +0.14 | +0.83 | −4 | −1 | +1 | +3 | +6 |
| ECMWF | 513 | +1.98 | 3.09 | −0.03 | +2.52 | −3 | 0 | +2 | +4 | +7 |
| Blend | 513 | +1.45 | 2.76 | +0.03 | +2.86 | −3 | 0 | +1 | +3 | +6 |

Both models have a warm bias (forecast > observed → positive residual meaning METAR is higher than forecast). GFS has lower bias but higher SD; ECMWF has higher bias but lower SD. The blend reduces SD to 2.76°F.

**By season (KLGA, day1):**

| Model | DJF bias/sd | MAM bias/sd | JJA bias/sd | SON bias/sd |
|-------|-----------|-----------|-----------|-----------|
| GFS | +1.29/2.73 | +1.60/3.56 | −0.43/3.43 | +0.54/2.58 |
| ECMWF | +2.93/2.53 | +2.18/3.12 | +1.39/3.61 | +1.14/2.54 |
| Blend | +2.17/2.24 | +1.89/2.76 | +0.47/3.22 | +0.96/2.21 |

ECMWF has a strong cold bias in DJF (+2.93°F means METAR was 2.93°F warmer than forecast). GFS has a slight warm bias in JJA (−0.43°F).

### EGLC (London) — day1 vintage

| Model | n | Bias | SD | Skew | Kurt | p5 | p25 | p50 | p75 | p95 |
|-------|---|------|----|----|------|----|----|----|----|-----|
| GFS | 528 | +0.70 | 2.30 | −0.34 | +1.41 | −3 | −1 | +1 | +2 | +4 |
| ECMWF | 513 | +0.97 | 2.17 | −0.49 | +1.36 | −3 | 0 | +1 | +2 | +4 |
| Blend | 513 | +0.83 | 1.99 | −0.41 | +0.88 | −3 | 0 | +1 | +2 | +4 |

London forecasts are more accurate (lower SD) with smaller biases. The blend at SD=1.99°F is notably tighter than NYC's 2.76°F.

### Mean Ranked Probability Score (in-sample events, Jul 2025–Mar 2026)

| Model | NYC Gaussian | NYC Empirical | London Gaussian | London Empirical |
|-------|-------------|---------------|-----------------|------------------|
| GFS | 0.1652 | 0.1657 | 0.1728 | 0.1801 |
| ECMWF | 0.1677 | 0.1672 | 0.1604 | 0.1612 |
| Blend | 0.1579 | 0.1576 | 0.1590 | 0.1638 |

The blend achieves the lowest RPS in NYC (0.158). In London, ECMWF is slightly better than GFS, and the blend is best (0.159). Gaussian and empirical methods produce very similar RPS.

---

## 4. Model-Run Release Schedule (Task 4)

| Model | Run (UTC) | NCEP/ECMWF availability | Open-Meteo update | Source |
|-------|-----------|------------------------|-------------------|--------|
| GFS | 00Z | ~05:12 UTC (±30 min) | "every 6 hours" (no exact lag) | [NCEP](https://www.nco.ncep.noaa.gov/pmb/nwprod/prodstat/) |
| GFS | 06Z | ~11:10 UTC (±30 min) | as above | NCEP |
| GFS | 12Z | ~17:12 UTC (±30 min) | as above | NCEP |
| GFS | 18Z | ~23:13 UTC (±30 min) | as above | NCEP |
| ECMWF IFS | 00Z | NA (end of dissemination schedule) | ~02:00 UTC (+2h lag stated) | [ECMWF](https://www.ecmwf.int/en/forecasts/datasets/open-data) |
| ECMWF IFS | 06Z | NA | ~08:00 UTC | ECMWF |
| ECMWF IFS | 12Z | NA | ~14:00 UTC | ECMWF |
| ECMWF IFS | 18Z | NA | ~20:00 UTC | ECMWF |

**NA rows:** ECMWF's official documentation states open data is "released at the end of the real-time dissemination schedule" but does not give an exact UTC time. Open-Meteo states "the open-data distribution has an additional delay of 2 hours compared to the real-time IFS dissemination."

**Open-Meteo update lag:** Open-Meteo's GFS page states updates are "every 6 hours" but does not document the exact lag after NCEP publication. The ECMWF page states a 2-hour additional delay for open-data vs real-time. These times carry ±30 minute uncertainty.

---

## 5. Kalshi Carry-Over (Task 5)

### 5a. KXHIGHNY market titles from Lychee dump

The Lychee dump's `kalshi/markets` table does not contain a `rules_primary` field. Only `title`, `yes_sub_title`, and `no_sub_title` are available. The title text is:

**Late 2024** (KXHIGHNY-24DEC31-T57):
> Will the **high temp in NYC** be >57° on Dec 31, 2024?
> yes_sub_title: 58° or above | no_sub_title: 58° or above

**Mid 2025** (KXHIGHNY-25JUN30-T93):
> Will the **high temp in NYC** be >93° on Jun 30, 2025?
> yes_sub_title: 94° or above | no_sub_title: 94° or above

**Nov 2025** (KXHIGHNY-25NOV17-T48):
> Will the **high temp in NYC** be >48° on Nov 17, 2025?
> yes_sub_title: 49° or above | no_sub_title: 49° or above

The station code (CLINYC) and settlement source (The Weather Company) were obtained from the **live Kalshi API** for current markets (Sep 2026). The dump titles mention only "NYC" — the station cannot be confirmed from historical data alone. It is possible that earlier markets used a different source (e.g., NWS — as seen for KXHIGHHOU).

### 5b. Settlement station comparison (7 overlap cities)

| City | Kalshi station | Kalshi source | PM station | PM source | Same station? |
|------|---------------|---------------|------------|-----------|---------------|
| NYC | CLINYC (Central Park) | The Weather Company | KLGA (LaGuardia) | Weather Underground | **NO** |
| Chicago | CLIMDW (Midway) | The Weather Company | KORD (O'Hare) | Weather Underground | **NO** |
| Austin | CLIAUS | The Weather Company | KAUS (Bergstrom) | Weather Underground | **NO** |
| Denver | CLIDEN | The Weather Company | KBKF (Buckley SFB) | Weather Underground | **NO** |
| Los Angeles | CLILAX | The Weather Company | KLAX (LAX) | Weather Underground | **NO** |
| Miami | CLIMIA | The Weather Company | KMIA (Miami Intl) | Weather Underground | **NO** |
| Philadelphia | CLIPHL | The Weather Company | not on Polymarket | — | N/A |

**No city uses the same station on both platforms.** Kalshi uses The Weather Company's station codes (CLI prefix, e.g. CLINYC = Central Park). Polymarket uses Weather Underground's ICAO airport stations. In NYC, the stations are ~8 miles apart.

---

## 6. Figures

### F1 — Trading window histograms

![NYC](../figs/F1_trading_window_new_york_city.png)
![London](../figs/F1_trading_window_london.png)

First trades cluster 24–48 hours before local midnight of settlement date. Last trades cluster around 27–28 hours after local midnight (i.e. 03:00–04:00 local the following day), with a secondary mode around 15:00–20:00 local on settlement day.

### F2 — Average path over the trading window

![NYC](../figs/F2_avg_path_new_york_city.png)
![London](../figs/F2_avg_path_london.png)

Implied SD declines through the window as uncertainty resolves. Trade counts peak around local midday on settlement day. The decline in SD accelerates after the modal daily-max hour (14:00 NYC, 13:00 London), consistent with the daily high becoming observable.

### F3 — Event-time overlays around model releases and METAR

![NYC](../figs/F3_event_time_new_york_city.png)
![London](../figs/F3_event_time_london.png)

Mean trade counts in 5-minute bins around GFS NCEP releases, ECMWF releases, and routine METAR observation times. The flat red line shows the window-wide mean for comparison.

### F4 — Market E[T] vs forecast daily max (day1 vintage)

![NYC](../figs/F4_scatter_new_york_city.png)
![London](../figs/F4_scatter_london.png)

Market expectations at local midnight of settlement day plotted against forecast daily max (day1 vintage). Points cluster tightly around the 45° line for all three models.

### F5 — Pre-sample residual histograms

![NYC](../figs/F5_residuals_new_york_city.png)
![London](../figs/F5_residuals_london.png)

Residual histograms (METAR − forecast, °F) for the pre-sample period (2024-01-01 to 2025-06-30). The Gaussian fit (red curve) overlaid on the empirical distribution. London residuals are visibly tighter than NYC.

### F6 — Market E[T] minus running METAR max on settlement day

![NYC](../figs/F6_gap_new_york_city.png)
![London](../figs/F6_gap_london.png)

The mean gap between the market's implied expected temperature and the running observed maximum shrinks through the settlement day. The gap crosses zero (market expectation equals observed max) around the modal daily-high hour, then continues declining as the market adjusts to the final observed value.

---

## 7. Not Verified

1. **Open-Meteo previous_day1 mapping to specific model runs.** The documentation states it represents "the value predicted 24 hours before valid time" but does not specify which initialisation cycle (00Z, 06Z, 12Z, 18Z) this corresponds to. This ambiguity means the day1 vintage may blend multiple runs.

2. **Open-Meteo update lag.** The exact time (in minutes) between NCEP/ECMWF product availability and Open-Meteo's update is not documented. This prevents precise event-study alignment. Figure F3 uses approximate NCEP release times, not Open-Meteo update times.

3. **ECMWF open-data dissemination times.** The ECMWF website states data is "released at the end of the real-time dissemination schedule" without giving exact UTC times. The schedule table records NA for these entries.

4. **Kalshi historical settlement source.** The Lychee dump does not include `rules_primary`. Station codes were obtained from the live API (Sep 2026) and may not reflect the source used for 2024–2025 markets.

5. **London Celsius bin matching in forecast RPS.** For events after ~Dec 11 2025, London bins are in °C. The Gaussian bin-probability computation converts bin bounds to °F for comparison with the °F-denominated error distribution. The empirical method was skipped for °C bins (n=319 vs 428 for London Gaussian RPS).

6. **Events without trades.** 158/432 NYC events and 156/430 London events had no trades in the eval_extract sample and are absent from the panel. They appear in the forecast RPS calculation (which does not require trades) but not in the trading-window or panel analyses.
