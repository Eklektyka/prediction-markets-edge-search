## Follow-up B — Synthetic Maker in Young Weather Markets

**Data:** Part 1 launch data (63 young °F events, age ≤14 days).
Blend forecast from Open-Meteo GFS+ECMWF at each station.
Error distribution: KLGA_blend_day1 (10000 historical errors, mean=1.42°F,
std=2.77°F) — used as proxy for all US °F cities (I4: fit ends before eval).
METAR daily max from IEM archive for running-max withdrawal.

**Resting-maker count:** Of 63 young events, 32 had any trades.
Median distinct makers per event: 2, mean: 11.0.
No-queue assumption justified: most young events have zero or one maker.

**Simulation:** At 12Z day−1, post bid/ask at blend-implied bin prices ± h.
Taker trades crossing our quote count as fills. Bins below observed running max: withdrawn.
No queue assumed. Scored to resolution.

**Results by city × age bucket:**

| City | Age | h | Fills | Gross | Adv sel | Events | Net/event |
|------|-----|---|-------|-------|---------|--------|-----------|
| Atlanta | 0-7 | 2c | 313 | $-1,744.27 | $2,099.03 | 8 | $-218.03 |
| Atlanta | 8-14 | 2c | 26 | $-157.09 | $362.60 | 7 | $-22.44 |
| Dallas | 0-7 | 2c | 282 | $-1,707.84 | $1,797.76 | 8 | $-213.48 |
| Dallas | 8-14 | 2c | 151 | $-2,818.76 | $3,171.65 | 7 | $-402.68 |
| New York City | 0-7 | 2c | 0 | $0.00 | $0.00 | 7 | $0.00 |
| New York City | 8-14 | 2c | 0 | $0.00 | $0.00 | 7 | $0.00 |
| Seattle | 0-7 | 2c | 161 | $-3,786.53 | $4,208.88 | 8 | $-473.32 |
| Seattle | 8-14 | 2c | 339 | $-2,747.12 | $3,000.29 | 7 | $-392.45 |
| Atlanta | 0-7 | 4c | 292 | $-1,476.10 | $2,082.15 | 8 | $-184.51 |
| Atlanta | 8-14 | 4c | 26 | $-117.52 | $362.60 | 7 | $-16.79 |
| Dallas | 0-7 | 4c | 282 | $-1,527.67 | $1,797.76 | 8 | $-190.96 |
| Dallas | 8-14 | 4c | 151 | $-2,452.13 | $3,171.65 | 7 | $-350.30 |
| New York City | 0-7 | 4c | 0 | $0.00 | $0.00 | 7 | $0.00 |
| New York City | 8-14 | 4c | 0 | $0.00 | $0.00 | 7 | $0.00 |
| Seattle | 0-7 | 4c | 161 | $-3,272.76 | $4,208.88 | 8 | $-409.09 |
| Seattle | 8-14 | 4c | 337 | $-2,391.65 | $2,999.59 | 7 | $-341.66 |

**T1–T5 (h = 4c, eval = full sample):**

- T1 (CI excludes zero): FAIL — CI [-332.0919, -78.9258]
- T2 (≥60% months positive): FAIL — 0/3
- T3 (concentration): PASS — top-10 event share 0.0%
- T4 (latency): PASS — maker/no queue
- T5 (capacity ≥ $100/mo): FAIL — $-3,746/mo

**Verdict: NOT TRADEABLE (2/5)**

**Not tested:** Hourly quote refresh with updated forecast vintages.
Quote withdrawal based on intraday METAR running max (only end-of-day used here).
Split by launch cohort.
