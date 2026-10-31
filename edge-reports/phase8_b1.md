## B1 — Top-Wallet Persistence on Realised Cash P&L

**Wallets analysed:** 101 (4 overlap between top-5 CP and top-100 by volume).
**Method:** FIFO round-trip cash P&L (buy then sell of same YES-token by same wallet). Open positions excluded.
**Trade-count floor:** 100 trades/month.
**Persistence test:** share of months with P&L > 0 vs within-wallet sign-permutation null (1,000 permutations).

### Top-5 Counterparties — Monthly Cash P&L

| Wallet | 2025-07 | 2025-08 | 2025-09 | 2025-10 | 2025-11 | 2025-12 | 2026-01 | 2026-02 | 2026-03 | 2026-04 | Total | Pos% | p-val |
|--------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-------|------|-------|
| 0xc8ab97a9… | $-2,331 | $-1,582 | $+3,256 | $-966 | $-8,549 | $-21,579 | $-45,589 | $-48,025 | $-1,836 | $+379 | $-126,822 | 20% | 0.992 |
| 0xea645729… | — | $-11,411 | $-38,602 | $-4,081 | — | — | — | — | — | — | $-54,094 | 0% | 1.000 |
| 0xc2e7800b… | — | — | — | — | — | — | $+0 | — | — | — | $+0 | — | — |
| 0xe90bec87… | — | — | — | — | $-43,910 | $-73,835 | $-15,200 | $+3,266 | $-5,560 | — | $-135,240 | 20% | 0.971 |
| 0xac44cb78… | — | — | — | — | — | — | — | $+0 | — | — | $+0 | — | — |

### Top-100 by Volume — Persistence Summary

| Metric | Value |
|--------|-------|
| Wallets with ≥3 valid months | 91 |
| Persistent at p < 0.05 (monthly) | 10 |
| Mean positive-month share | 57.4% |
| Mean total cash P&L | $+33,469 |
| Median total cash P&L | $+222 |

### Quarterly Persistence

Top-100 wallets with ≥50% positive quarters (of those with ≥2 quarters): 50 / 85

### Chess Maker Cross-Check

Chess market-maker (0xC5d5…f80a, $23B volume): **NOT** in top-5 CPs or top-100 by volume.
This wallet's activity is concentrated in chess markets (0.08% of volume), so it does not appear in the general top-counterparty list.

### Result

**Top-5 counterparties:** 0/5 persist at p < 0.05.
**Top-100 by volume:** 10/100 persist at p < 0.05.
