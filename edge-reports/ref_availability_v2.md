# Reference-Availability Test v2 — Market Level

_Generated: 2026-09-24 00:59 UTC_
_Jul 2025 – Jan 2026. 58,528 resolved markets with trades._

## 1. Reference Classification

| Category | Sharp exchange | Sharp book | Bookmaker | Niche book | Polls | NWP | None | Total |
|----------|--------------|-----------|-----------|-----------|-------|-----|------|-------|
| Crypto | 145,245 | 0 | 0 | 0 | 0 | 0 | 0 | 145,245 |
| Culture | 0 | 0 | 889 | 0 | 0 | 0 | 6,205 | 7,094 |
| Finance | 8,220 | 0 | 0 | 0 | 0 | 0 | 61 | 8,281 |
| Politics | 0 | 0 | 0 | 0 | 2,341 | 0 | 6,457 | 8,798 |
| Sports | 0 | 105,558 | 0 | 33,368 | 0 | 0 | 0 | 138,926 |
| Tech | 0 | 0 | 0 | 0 | 0 | 0 | 1,754 | 1,754 |
| Weather | 0 | 0 | 0 | 0 | 0 | 5,935 | 0 | 5,935 |

## 2. Market Quality by Reference Class

| Reference | N | Volume | Brier | Reliability | Coherence | Absorption (s) | C0 share |
|-----------|---|--------|-------|-------------|-----------|---------------|----------|
| Sharp exchange | 21,613 | $335M | 0.0067 | 0.00053 | 0.079 | 14854 | 2.1% |
| Sharp sportsbook | 13,929 | $593M | 0.0136 | 0.00090 | 0.115 | 7234 | 9.4% |
| Bookmaker odds | 660 | $1M | 0.0095 | 0.00016 | 0.015 | 7616 | 4.0% |
| Niche/thin book | 5,813 | $85M | 0.0087 | 0.00072 | 0.074 | 6878 | 7.5% |
| Polls | 1,959 | $80M | 0.0060 | 0.00015 | 0.034 | 19558 | 2.6% |
| NWP/METAR | 3,706 | $4M | 0.0011 | 0.00012 | 0.053 | 21346 | 2.4% |
| None | 10,848 | $336M | 0.0095 | 0.00061 | 0.047 | 16534 | 4.1% |

## 3. Regression: Quality ~ Reference + Category FE + Volume FE

Event-clustered standard errors. `ref_order`: 0=sharp exchange … 6=none (higher = less sharp).

| Dependent | ref_order coef | SE | t | R² | Interpretation |
|-----------|---------------|-----|---|-----|----------------|
| Brier | -0.001355 | 0.000444 | -3.05 | 0.0061 | Less sharp → lower brier (significant) |
| Coherence | +0.001631 | 0.001296 | +1.26 | 0.0701 | Less sharp → higher coherence (not significant) |
| C0 share | -0.000419 | 0.000962 | -0.44 | 0.2118 | Less sharp → lower c0 share (not significant) |

## Within-Category Contrasts

| Contrast | N (A/B) | Brier A | Brier B | Δ Brier | Coh A | Coh B | C0 A | C0 B |
|----------|---------|---------|---------|---------|-------|-------|------|------|
| Sports: sharp vs niche | 13929/5813 | 0.0136 | 0.0087 | +0.0050 | 0.115 | 0.074 | 9.4% | 7.5% |
| Culture: bookmaker-priced vs not | 660/4841 | 0.0095 | 0.0079 | +0.0016 | 0.015 | 0.033 | 4.0% | 3.6% |
| Politics: polled vs not | 1959/4528 | 0.0060 | 0.0116 | -0.0057 | 0.034 | 0.066 | 2.6% | 5.1% |

## 4. Figure: Mean Brier by Reference Class (volume-adjusted)

```
  Sharp exchange     ██░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 0.0067 (N=21,613)
  Sharp sportsbook   ████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 0.0136 (N=13,929)
  Bookmaker odds     ███░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 0.0095 (N=660)
  Niche/thin book    ██░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 0.0087 (N=5,813)
  Polls              █░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 0.0060 (N=1,959)
  NWP/METAR          ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 0.0011 (N=3,706)
  None               ███░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 0.0095 (N=10,848)
```

**Spearman** (ref order vs mean Brier, N=7 classes): ρ = -0.214, p = 0.645

**Reversed** — markets without references have LOWER Brier (better calibration). This may reflect base-rate differences: no-reference markets tend to resolve at extreme prices (near 0 or 1), mechanically lowering Brier.
