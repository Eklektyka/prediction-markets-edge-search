# PREREG.md — Polymarket copy-trading feasibility, Phases 2–3

Frozen BEFORE any Phase 2 code is written or any EVAL-window fill is
examined. Amendments after this commit: none. Deviations forced by data
problems are logged in qa/ with justification and marked as deviations.

## Data and conventions

- Dataset: vgregoire/polymarket-users v1.0 (2026-05-19), CC-BY 4.0.
  Cite Akey, Grégoire, Harvie & Martineau (2026, SSRN 6443103).
- Windows: TRAIN = sample start → 2025-06-30; EVAL = 2025-07-01 →
  2026-03-29. Disjoint. All ranking inputs are TRAIN-only.
- pnl_change panels label periods at the right boundary (+1 day).
- HF `price` is TOKEN-NATIVE. All price paths and volume sums are
  computed in token space (price × quantity); Yes-equivalent
  conversion via the outcome column only where legs are compared.
- Quantities reconcile 1:1 with exchange API fills (Phase 0c). No
  dedup rule is applied to sums; multi-row tx are multi-maker splits
  and genuine two-sided fills.

## Signal wallet sets (frozen file: data/interim/top_wallets_train.parquet)

- SHA256: 44b950ff49a1f787ca81bf2a3f30bc522c4a7aa383079fe1edc411932c8b67dd
- Eligibility floors (in order): ≥20 resolved positions; ≥6 active
  TRAIN months; wash exclusion (counterparty_hhi ≥ 0.5 AND ≥100
  trades).
- PRIMARY: top-1% by t-stat of monthly TRAIN PnL among eligible
  wallets with TRAIN summed PnL ≥ $1,000.
- Descriptive arms (reported, never gating): ARM_T (t-stat, no
  floor), ARM_SUM (summed PnL), ARM_BOTH (intersection).
- CONTROL: wallets ranked 45th–55th percentile on the t-stat among
  eligible wallets (no PnL floor).
- Validation gate (passed, Phase 0): top-1% of positive-PnL users
  hold 76.5% of summed gains on pnl_total — matches paper.

## Phase 2 — post-fill drift event study (descriptive, not gating)

- Sample: all EVAL-window fills by PRIMARY and CONTROL wallets.
- Outcome: signed token-space price change of the traded token at
  +1m, +5m, +30m, and to resolution, sign = direction of the signal
  wallet's position (buy = +).
- Estimand: PRIMARY minus CONTROL mean signed drift per horizon.
- Pre-specified cuts (the only ones feeding interpretation):
  (a) maker vs taker side of the signal wallet's fill;
  (b) the four set arms.
- Inference: cluster by event; two-stage bootstrap (precompute
  per-event stats, resample events).
- Robustness (reported, non-gating): cluster-by-wallet SEs and a
  leave-one-wallet-out range for all PRIMARY estimates, Phases 2–3.
- Data handling: download EVAL day partitions sequentially, filter to
  signal/control wallets' fills + the traded markets' trades, persist
  the slim extract, delete raw partitions. No day sampling.

## Phase 3 — copyability overlay (GATING)

- Copied-entry price: first trade in the same token strictly after
  the signal fill's timestamp (any wallet). No same-timestamp entry.
- Exit: resolution payoff of the token.
- Costs: taker fee where markets.has_fee = True; fee-free otherwise.
- Metric: mean net edge per copied trade (cents, token space),
  PRIMARY set, ALL fills pooled (lagged entry itself prices the
  copyability of maker fills). Maker/taker split reported alongside.
- Capacity proxy: traded volume in the 30 min following each signal
  fill.

## KILL RULE (verbatim commitment)

If mean net edge per copied trade — lagged entry, net of fees,
PRIMARY set, EVAL window — is not positive with cluster-robust
|t| ≥ 2, the live agent is NOT built. The result is written up as a
tradability null. No arm, cut, horizon, or subset result overrides
this. Passing on any descriptive arm while PRIMARY fails = not built.

## Priors on record (for honesty in write-up)

- Author's Kalshi result: taker flow predicts sub-fee reversal.
- Akey et al.: winners provide liquidity; persistence modest,
  possibly selection. Vedova: profits from execution, not
  information. Expected outcome: null.
