# Track B Order-Flow Reversal — closed 2026-07-14

**Question:** Does signed taker order-flow imbalance in one 5-min interval
predict the next interval's price revision?

**What I expected:** Confirmatory criterion (qa/trackB_confirmatory_score.md
line 18): "spread < 0 AND |t| >= 2."

**What I found:** Statistically confirmed, economically dead.

- Volume-scaled OFI Q5–Q1 spread (lag-1, cents): −1.1265.
  t-statistic (SE clustered by fight card): −6.29. p < 0.0001.
  Verdict: CONFIRM.
  (`qa/trackB_confirmatory_score.md` lines 16–18)

- Sample: Feb–Nov 2025 Kalshi UFC, 26 fight cards, 483,424 trades.

- Round-trip cost at p = 0.45: Kalshi taker fee 3.47¢ + spread ~2¢ = ~5.5¢.
  Signal reversal = 1.13¢. Sub-fee by ~4×.

**Why I stopped:** The effect is real but the cost to capture it is 4× the
signal. This is a structural finding about market microstructure (taker flow
predicts short-term reversals), not a tradeable strategy.

**What it rules out:** Short-horizon OFI momentum/reversal strategies on
Kalshi UFC markets are not viable at current fee levels.

**What would reopen it:** Fee reduction to below ~0.5¢ round-trip (currently
~5.5¢). Not foreseeable.
