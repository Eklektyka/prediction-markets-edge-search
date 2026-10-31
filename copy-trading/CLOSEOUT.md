# Machine Search Close-out

_Investigation: Polymarket copy-trading and maker feasibility study._
_Capital deployed: $0. All analysis offline or shadow-only._
_Closed: 2026-08-13._

## Seven-habitat ledger

| # | Habitat | Gate | Verdict | Key commit |
|---|---------|------|---------|------------|
| 1 | **Taker copy-trading** (follow PRIMARY wallets) | Net edge per copied trade ≥ 0, \|t\| ≥ 2 | **PASS** (+0.0016, t=4.13) but spread-infeasible at retail | `ccf8de0` |
| 2 | **Maker copy** (post at signal wallet's price) | Pessimistic fill edge > 0 | **FAIL** — unfilled orders carry +0.15–0.48 edge; adverse selection on fills | `71d7a40` |
| 3 | **Kalshi fade-quoting** (Track B OFI cross-venue) | Edge > Kalshi fees (7% × P(1-P)) | **FAIL** — pessimistic +0.0014, CI spans zero | `acb9730` |
| 4 | **Polymarket maker, Politics <50/12-24** | Resolution markout CI excl zero, ≥ +0.005 | **PASS** offline (+0.0056) but **ERODED** to +0.0010 CI spans zero in Apr-Aug backfill | `8e4e4da` |
| 5 | **Polymarket maker, Weather >500/12-24** | Resolution markout CI excl zero, ≥ +0.005 | **FAIL** — CI spans zero (+0.0039 [-0.0003, +0.0091]) | `5d58753` |
| 6 | **Weather conditioned cell** (UTC 03-16, ≤12h settle) | Hypothesis-setting only | +0.0018 [+0.0003, +0.0034] — positive but **not pre-registered**, shadow-only | `3fa6b92` |
| 7 | **Shadow quoter (Stage A)** | 30-day shadow with QUEUE markout CI excl zero | **Day 1 negative** (W: -0.026); insufficient accumulation to score | `94a0508` |

**Result: 7/7 gates failed or eroded. No live deployment.**

## Methodology caveats

### 1. Block-time vs taker-time staleness
The data-api `timestamp` field is block-confirmation time (~2-15s inflation
vs on-chain event logs in the HF dataset). The <3s staleness bucket that
drove the offline winning segment is empty in the backfill data. The 3-10s
bucket is the closest proxy, after adjusting for the ~3-5s offset. Direct
numeric comparison of staleness-conditioned edges between the offline study
(HF data) and backfill studies (data-api) requires this adjustment.

### 2. Gamma tag ≠ HF category
The Gamma API `tag=Politics` returns ITF tennis, Dota 2, baseball O/U, and
Ethereum price brackets — NOT the HF dataset's political markets. Two
backfill attempts (politics_backfill.py) were discarded after this
population mismatch was identified. The true-population backfill v3 used
HF `markets.parquet` `category=Politics` as the anchor, resolving
`conditionId` per market via Gamma `/markets/<id>`.

## Decay result

The one habitat that passed its gate offline (Politics <50/12-24, +0.0056)
showed **83% edge compression** in the Apr-Aug 2026 backfill:

| Period | Mean markout | 95% CI | N |
|--------|-------------|--------|---|
| Feb-Mar 2026 (offline) | +0.0056 | [+0.0039, +0.0076] | 588K |
| Apr-Aug 2026 (backfill v3) | +0.0010 | [-0.0017, +0.0038] | 450K |

The edge is not statistically distinguishable from zero in the later
period. This is consistent with market maturation: as Polymarket's maker
population grows, the informational advantage of quoting in quiet segments
erodes.

## Capital spent

$0. No live orders were placed. All analysis was offline (historical data)
or shadow-only (virtual quotes on live feeds, no capital at risk).

## Unexplored branch (backlog)

**Kalshi maker environment**: The Becker data dump + Kalshi API backfill
route was identified but not pursued. The Kalshi fee structure (7% × P(1-P)
per side) creates a high bar, and the Phase 4c fade-quoting test already
failed against this bar. Prior: low. No clock.

## Archive

- `data/archive/logs/` — shadow collector JSONL (book, trades, universe)
- `data/archive/july_backfill/` — W+P weather/politics backfill data
- `data/archive/politics_backfill_v3/` — HF-anchored P backfill data
- See `data/archive/README.md` for dates and provenance.
