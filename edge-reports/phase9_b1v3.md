## B1v3 — Settled-P&L Persistence

**Wallets analysed:** 101 (4 overlap between top-5 CP and top-100 by volume).
**Method:** Settled P&L = closed round-trips (FIFO, per asset) + resolved positions (payout − cost basis, attributed to resolution month). Positions in unresolved markets excluded.
**Excluded notional share:** 7.6% of open-position notional is in unresolved markets.
**Trade-count floor:** 100 trades/month.
**Persistence test:** share of months with settled P&L > 0 vs within-wallet sign-permutation null (1,000 permutations).

### Top-5 Counterparties — Monthly Settled P&L (round-trip / resolution split)

| Wallet | Months | RT P&L | Res P&L | Settled P&L | Pos% | p-val |
|--------|--------|--------|---------|-------------|------|-------|
| 0xac44cb78… | 1 | $+0 | $+1,523,365 | $+1,523,365 | — | — |
| 0xc2e7800b… | 1 | $+0 | $+1,452,123 | $+1,452,123 | — | — |
| 0xe90bec87… | 5 | $-135,240 | $+697,430 | $+562,190 | 60% | 0.506 |
| 0xea645729… | 3 | $-54,094 | $+61,659 | $+7,565 | 33% | 0.864 |
| 0xc8ab97a9… | 10 | $-126,822 | $+52,574 | $-74,248 | 50% | 0.639 |

**Raw monthly series (top-5):**

**0xac44cb78…:**

| Month | RT P&L | Res P&L | Settled | Trades |
|-------|--------|---------|---------|--------|
| 2026-02 | $+0 | $+1,523,365 | $+1,523,365 | ≥100 |

**0xc2e7800b…:**

| Month | RT P&L | Res P&L | Settled | Trades |
|-------|--------|---------|---------|--------|
| 2026-01 | $+0 | $+1,452,123 | $+1,452,123 | ≥100 |

**0xe90bec87…:**

| Month | RT P&L | Res P&L | Settled | Trades |
|-------|--------|---------|---------|--------|
| 2025-11 | $-43,910 | $+172,648 | $+128,738 | ≥100 |
| 2025-12 | $-73,835 | $+509,420 | $+435,585 | ≥100 |
| 2026-01 | $-15,200 | $-42,716 | $-57,917 | ≥100 |
| 2026-02 | $+3,266 | $+138,485 | $+141,751 | ≥100 |
| 2026-03 | $-5,560 | $-80,408 | $-85,967 | ≥100 |

**0xea645729…:**

| Month | RT P&L | Res P&L | Settled | Trades |
|-------|--------|---------|---------|--------|
| 2025-08 | $-11,411 | $+30,405 | $+18,994 | ≥100 |
| 2025-09 | $-38,602 | $+27,401 | $-11,201 | ≥100 |
| 2025-10 | $-4,081 | $+3,854 | $-227 | ≥100 |

**0xc8ab97a9…:**

| Month | RT P&L | Res P&L | Settled | Trades |
|-------|--------|---------|---------|--------|
| 2025-07 | $-2,331 | $+3,635 | $+1,304 | ≥100 |
| 2025-08 | $-1,582 | $-5,907 | $-7,489 | ≥100 |
| 2025-09 | $+3,256 | $+6,366 | $+9,622 | ≥100 |
| 2025-10 | $-966 | $+7,882 | $+6,916 | ≥100 |
| 2025-11 | $-8,549 | $+4,429 | $-4,120 | ≥100 |
| 2025-12 | $-21,579 | $+13,213 | $-8,366 | ≥100 |
| 2026-01 | $-45,589 | $-26,312 | $-71,902 | ≥100 |
| 2026-02 | $-48,025 | $+29,589 | $-18,436 | ≥100 |
| 2026-03 | $-1,836 | $+19,546 | $+17,710 | ≥100 |
| 2026-04 | $+379 | $+133 | $+512 | ≥100 |

### Top-100 by Volume — Persistence Summary

| Metric | Value |
|--------|-------|
| Wallets with ≥3 valid months | 91 |
| Persistent at p < 0.05 (monthly) | 11 |
| Mean positive-month share | 61.7% |
| Mean total settled P&L | $+31,576 |
| Median total settled P&L | $+34,367 |
| Mean RT P&L | $+33,469 |
| Mean resolution P&L | $-1,893 |

### Quarterly Persistence

Top-100 wallets with ≥50% positive quarters (of those with ≥2 quarters): 65 / 85

Quarterly persistent at p < 0.05: 0 / 85

### Chess Maker Cross-Check

Chess market-maker (0xC5d5…f80a): **NOT** in top-5 CPs, **NOT** in top-100 vol.

### Result

**Top-5 counterparties:** 0/5 persist at p < 0.05 on settled P&L.
**Top-100 by volume:** 11/100 persist at p < 0.05 (monthly).
**Excluded notional:** 7.6% of open-position notional in unresolved markets.
**I6:** Σ monthly settled = total settled for all wallets. PASS.
