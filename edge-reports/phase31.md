# Phase 31 — Following the Sharps' First Fill

_Generated: 2026-09-20 19:48 UTC_
_Cluster-0 wallets: 10137. Window: Jul 2025 – Jan 2026._

## 1. Signals

**Definition:** First c0 fill on a side, ≥50 contracts, no c0 fill on same side in prior 30 min.
**Total signals:** 74084
**Signals/day:** 346.2

## 2. Move Anatomy

**Mean move (signal direction, +5min):** -0.06c
**Positive direction share:** 29%

| Latency | Residual gap (median) | Capacity (median contracts) |
|---------|---------------------|---------------------------|
| 2s | 0% | 174 |
| 5s | 0% | 183 |
| 10s | 0% | 191 |
| 30s | 0% | 201 |
| 60s | 0% | 229 |

## 3. Strategy

_(See log output for per-latency results)_

## 4. Signal Quality

**Precision (≥2c move within 5min):** 11%
**Top-100 wallets precision:** 41%
**Top-100 mean move:** +2.35c vs all -0.06c

## 5. Persistence

**Persistent wallets (≥3 months):** 1800/7125 (25%)
**Persistent volume share:** 62%

## 6. Followers

**Non-c0 fills within 10s of signal:** 713
**Follower P&L:** median $+0.0, mean $+2.2

## 7. Verdict

**Residual at 5s:** 0% of the move remains. **Precision:** 11% of signals produce a ≥2c move. **Crowding:** 713 follower fills within 10s. **Persistence:** 25% of signal wallets stay active ≥3 months (62% of volume). A $2,000 trader following sharp first fills at 5s latency would capture 0% of each move, with 11% of signals being correct. The strategy is not feasible — the move is consumed before 5s.
