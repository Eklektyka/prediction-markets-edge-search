## P3 Fix — Persistence Basis Confirmation

**Current basis:** Resolution mark-to-market (PnL = Σ (resolution_value − fill_price) × qty).
This is from agg_E, which scores each trade to its market's final resolution.

**Required basis:** Realised cash P&L on closed round-trips per asset ID (I10).
A round-trip = buy then sell (or sell then buy) on the same prediction_id.
Cash PnL = sell_price − buy_price per contract (no resolution needed).

**Gap:** agg_E does NOT contain round-trip data. It scores to resolution,
which includes unrealised position changes. A wallet that bought at 0.50 and
the market resolved YES gets +0.50/c — but they never sold, so their cash
flow was −0.50 (buy) + 1.00 (resolution payout) = +0.50. For markets that
haven't resolved, resolution MTM is undefined.

**To fix properly:** Re-scan eval_extract tracking position per
(taker_address, prediction_id). For each trade, update the running position.
When position flips sign or reaches zero, close the round-trip and compute
cash PnL. This is a ~10-minute stage-1 extension.

**Current result stands as:** Resolution-based persistence (not cash round-trips).
The finding (0 of 92 persist at p < 0.05) would likely be similar or weaker on
cash P&L, since resolution MTM is a superset of realised cash.

**Status:** Resolution-based result reported; cash round-trip requires dedicated scan.
