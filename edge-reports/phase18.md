# Phase 18 — Is the Top Quartile Skill or Luck?

_Generated: 2026-09-16 12:52 UTC_

## 1. Persistence

**Wallets with ≥2 quarters:** 2954
**Quarters:** 2025-Q3, 2025-Q4, 2026-Q1

**Transition Matrix (row = Q, column = Q+1):**

| From \ To | Q1 (bot) | Q2 | Q3 | Q4 (top) |
|-----------|---------|----|----|---------|
| Q1 (bot) | 56% | 23% | 11% | 11% |
| Q2 | 24% | 37% | 23% | 17% |
| Q3 | 11% | 24% | 40% | 26% |
| Q4 (top) | 7% | 18% | 26% | 49% |

**Spearman ρ (consecutive quarter returns):** 0.497 (p=0.0000)
**Permutation p-value:** 0.000

**Returns are persistent** — top-quartile makers tend to stay in the top quartile more often than chance would predict. This suggests skill, not luck.

## 2. Fingerprint: Top vs Bottom Quartile

**Top quartile (Q4):** 739 wallets
**Bottom quartile (Q1):** 738 wallets

| Feature | Top Q4 | Bottom Q1 | Diff | p-value |
|---------|--------|-----------|------|---------|
| fills_per_q | 2028.25 | 1493.62 | +534.63 | 0.098 |
| contracts_per_q | 460072.43 | 372661.92 | +87410.51 | 0.443 |
| reward_share | 0.09 | -0.01 | +0.09 | 0.000 |
| capital | 25890.74 | 201656.06 | -175765.32 | 0.000 |
| n_markets | 63.99 | 51.05 | +12.95 | 0.218 |
| total_rewards | 322.03 | 220.91 | +101.13 | 0.000 |
| total_spread | 25644.21 | -456352.71 | +481996.92 | 0.000 |

**Top distinguishing features:**
- **reward_share:** top=0.09 vs bot=-0.01 (p=0.000)
- **total_spread:** top=25644.21 vs bot=-456352.71 (p=0.000)
- **capital:** top=25890.74 vs bot=201656.06 (p=0.000)
- **total_rewards:** top=322.03 vs bot=220.91 (p=0.000)
- **fills_per_q:** top=2028.25 vs bot=1493.62 (p=0.098)

## 3. Rule Replication

**Rules derived from fingerprint:**
- Prioritize reward income over spread (select high-rate markets)
- Better spread capture (tighter quotes, better market selection)
- Deploy less capital per market (smaller quotes)

| Metric | Without rules | With rules |
|--------|-------------|-----------|
| Markets simulated | 933 | 861 |
| Portfolio median $/mo | $2 | $2 |
| Portfolio P5 $/mo | $-8 | $-10 |
| Portfolio P95 $/mo | $13 | $15 |

**T1–T5 (with rules):**
- T1 (P5 > 0): FAIL
- T5 (≥$100/mo): FAIL — median $2

## 4. Verdict

**Skill, not luck.** Quarterly returns show persistence (ρ=0.497, p=0.000). The top quartile differs from the bottom on 4 features at p<0.05. The most discriminating feature is **reward_share**. Encoding the top-quartile rules in the synthetic maker does not improve the median portfolio from $2/mo to $2/mo. The improvement is insufficient to reach T5 ($100/mo).
