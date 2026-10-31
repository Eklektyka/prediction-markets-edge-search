## Part 3 — Top Wallet Persistence

**Method:** Monthly resolution-markout series for top-100 wallets by volume
and Part 6's top-5 counterparties. Trade-count floor: vol ≥ 100/month
(zero-fill otherwise). Within-wallet permutation null (1,000 shuffles).
Monthly AND quarterly granularity.

**Results:**

- Tested: 92 wallets
- Persist at p < 0.05 (monthly): **0** (expected under null: ~4)
- Persist at p < 0.05 (quarterly): **0**

| Addr | Pos month% | p (monthly) | Pos quarter% | p (quarterly) | Active months | PnL | Volume | Note |
|------|-----------|-------------|-------------|---------------|--------------|-----|--------|------|
| W4f0d5be6 | 55.6% | 1.000 | 66.7% | 1.000 | 5 | $8,337 | 4,468,001 |  |
| W32c5ca4f | 55.6% | 1.000 | 66.7% | 0.726 | 9 | $4,616 | 5,714,397 |  |
| W6794dd12 | 33.3% | 1.000 | 33.3% | 0.752 | 9 | $-13,608 | 13,764,574 |  |
| Wef7cfcb7 | 66.7% | 1.000 | 33.3% | 1.000 | 8 | $-97,564 | 11,000,552 |  |
| W2da35e62 | 44.4% | 1.000 | 33.3% | 1.000 | 9 | $-19,115 | 5,232,411 |  |
| Wc4aff144 | 66.7% | 1.000 | 100.0% | 0.765 | 8 | $29,165 | 6,775,156 |  |
| W9054a59b | 11.1% | 1.000 | 0.0% | 1.000 | 9 | $-20,761 | 15,286,276 |  |
| W77ce4e57 | 33.3% | 1.000 | 0.0% | 1.000 | 9 | $-21,759 | 8,152,060 |  |
| W376a7736 | 11.1% | 1.000 | 33.3% | 0.540 | 5 | $-19,191 | 9,433,356 |  |
| W9bd27ccc | 22.2% | 1.000 | 33.3% | 1.000 | 3 | $88,549 | 9,294,188 |  |
| W4b5eaa4a | 33.3% | 1.000 | 33.3% | 0.956 | 6 | $44,070 | 7,137,225 |  |
| W0341b149 | 33.3% | 1.000 | 33.3% | 0.964 | 9 | $-488,502 | 84,012,384 |  |
| We9b45768 | 11.1% | 1.000 | 33.3% | 0.758 | 4 | $-1,287 | 22,513,696 |  |
| Wea4661cc | 22.2% | 1.000 | 33.3% | 1.000 | 6 | $-26,388 | 6,411,116 |  |
| W9d28ce41 | 77.8% | 1.000 | 66.7% | 1.000 | 9 | $120,026 | 8,372,506 |  |
| Wc8109fad | 22.2% | 1.000 | 66.7% | 0.752 | 3 | $-5,989 | 5,714,770 |  |
| W13e5d544 | 66.7% | 1.000 | 66.7% | 0.783 | 9 | $99,133 | 34,277,016 |  |
| W2fcc1f7d | 22.2% | 1.000 | 33.3% | 0.879 | 4 | $845 | 18,545,587 |  |
| W062df67d | 44.4% | 1.000 | 66.7% | 0.935 | 6 | $30,956 | 11,009,036 |  |
| Wbe2b5014 | 22.2% | 1.000 | 33.3% | 1.000 | 3 | $144,359 | 5,199,332 |  |
| W0df762d2 | 44.4% | 1.000 | 66.7% | 0.507 | 8 | $68,811 | 4,936,588 |  |
| W3c1cd128 | 22.2% | 1.000 | 33.3% | 1.000 | 6 | $-14,559 | 5,919,448 |  |
| W175dd5d7 | 11.1% | 1.000 | 33.3% | 0.619 | 7 | $-5,449 | 5,522,841 |  |
| We7bd7918 | 44.4% | 1.000 | 33.3% | 0.968 | 9 | $-34,511 | 57,318,301 |  |
| W5497a0c7 | 22.2% | 1.000 | 33.3% | 1.000 | 3 | $17,784 | 4,411,734 |  |
| W1e9da237 | 11.1% | 1.000 | 0.0% | 1.000 | 3 | $-4,536 | 8,193,948 |  |
| W536b0b57 | 66.7% | 1.000 | 66.7% | 1.000 | 9 | $59,703 | 5,411,711 |  |
| W64a02e9e | 66.7% | 1.000 | 66.7% | 0.851 | 9 | $18,860 | 11,778,505 |  |
| W8f050e68 | 44.4% | 1.000 | 33.3% | 1.000 | 5 | $65,123 | 6,914,139 |  |
| W4b8c0d28 | 44.4% | 1.000 | 33.3% | 0.450 | 9 | $-4,370 | 6,016,898 |  |

### Top-5 counterparties

- W4a46a5cf: monthly p=1.000, quarterly p=0.960, PnL=$-130,138
- Wfefc78ae: monthly p=1.000, quarterly p=0.969, PnL=$150,263
- Wa431e0ea: monthly p=1.000, quarterly p=1.000, PnL=$-402,111

### Chess wallet cross-check

Chess-related files found: ['chess_exploratory.md', 'chess_ids.csv', 'chess_markets.parquet'].
The $23B-volume chess market-maker address cannot be verified against Part 6's
top-5 without the raw (un-anonymised) address from the chess analysis outputs.
A targeted match would require loading the chess_markets raw data and comparing
raw addresses.
