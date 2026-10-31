# Prediction Markets Edge Search

I searched Polymarket and Kalshi for an edge a retail trader could actually capture: 25 strategies across politics, sports, weather and crypto markets, each run against a kill rule written down before I saw the result.

**Status:** Parked — two candidates remain under evaluation (markets, rules and results withheld until out-of-sample testing is complete). Every other strategy is closed, with the reason recorded below.

## At a glance

| | | Source |
|---|---|---|
| Polymarket trades in the historical dump | ~405 million rows | report withheld (internal) |
| Kalshi trades in the historical dump | ~72 million rows | report withheld (internal) |
| Core dataset (YES-leg, deduplicated) | 62.7 million trades, 729K markets | [`edge-reports/phase9_report.md`](edge-reports/phase9_report.md) |
| Phase reports on disk | 60 | counted from edge/report/ |
| Strategies tested | 25 | |
| Data bugs caught before they became a "finding" | 8 (see below) | |

## What I built

- A validated core dataset of single-leg trades and markets, with hard assertions that stop a run on any integrity failure (price must exist at decision time, no fallbacks, no look-ahead).
- Polymarket fill extraction from on-chain `OrderFilled` events and the CLOB data API, with the YES/NO complement-leg duplication removed and verified 1:1 against exchange records.
- Kalshi collection via the RSA-PSS authenticated API, plus a forward order-book collector.
- Weather joins: Polymarket daily-high markets against METAR observations (IEM ASOS archive) and GFS/ECMWF forecast runs (Open-Meteo).
- Shared metric primitives (FIFO positions, holding time, markout vs pre-fill mid, spread capture) with unit tests, so every analysis uses one definition.

Stack: Python, polars, DuckDB, parquet, pytest.

## What I tested, and why it died

**Fast strategies (latency or information)**

| Strategy | Idea | Verdict | Why it died | Key number | Source |
|---|---|---|---|---|---|
| Weather observation reaction | Sell bins made impossible by a new daily-high reading | Dead | Fastest participant reacts 3–6 s after the observation; residual liquidity is negative-EV | 3–6 s | [`weather-reports/estimate.md`](weather-reports/estimate.md) |
| Weather forecast edge at the open | Trade the open against a bias-corrected GFS/ECMWF blend | Dead | Open already calibrated to the forecast blend within 5 percentage points per bin | zero signals | report withheld (internal) |
| Buy locked weather winners | Buy the bin once the day's max is "locked" | Dead | A later higher reading arrives 5.3% of the time; fails breakeven, fatal in winter | 5.3% | [`weather-reports/a2_followup.md`](weather-reports/a2_followup.md) |
| 15-minute crypto fair value | Quote crypto markets off a live fair value | Dead | Zero first-mover fills | $0 | [`edge-reports/phase13_tradeability.md`](edge-reports/phase13_tradeability.md) |
| Stale-quote sniping (chess) | Hit stale quotes during the 2024 World Championship | Dead | One maker in the market; total stale-quote prize for the whole event | $531 | [`edge-reports/thin_market_map.md`](edge-reports/thin_market_map.md) |
| Following sharp wallets | Copy the first fill of identified sharp takers | Dead | No subsequent move to follow | — | |
| Pre-game sportsbook lag | Trade Polymarket soccer against the Pinnacle close | Dead | Prices within 2¢ of Pinnacle for the pre-game window | mean gap −0.17¢ | [`edge-reports/phase32_convergence.md`](edge-reports/phase32_convergence.md) |
| UFC sportsbook lag | Buy the prediction-market side that is cheap vs the book | Dead | Book-price feed had 6–54 h capture latency; edge bracketed between ~0% (implementable) and +6–8% (look-ahead) | ~0% to +6–8% | report withheld (internal) |

**Slow strategies (premia and structure)**

| Strategy | Idea | Verdict | Why it died | Key number | Source |
|---|---|---|---|---|---|
| Longshot selling, Polymarket | Sell low-probability contracts on a zero-fee venue | Dead | Positive but tiny: ~3.8% a year on locked capital | 3.8%/yr | [`edge-reports/phase13_tradeability.md`](edge-reports/phase13_tradeability.md) |
| Longshot selling, Kalshi | Same, with fees | Dead | Premium exists but the round-trip fee eats it | — | |
| Crypto threshold short vol | Sell deep OTM price-threshold contracts | Dead | Only 34 qualifying fills in the ≤10¢ bucket; short-volatility position with unmeasured tail | n=34 | report withheld (internal) |
| Multi-outcome longshot basket | Buy or sell baskets of bounded outcomes | Dead | First result was a bug (93% of entries on markets not yet created); corrected sample too small | 93% invalid | [`edge-reports/p2_bugcheck.md`](edge-reports/p2_bugcheck.md) |
| Young-market making | Quote newly listed markets before bots arrive | Dead | Markets are untraded, and the few takers are informed; synthetic maker loses everywhere | — | |
| Liquidity rewards | Earn the venue's maker rewards on resting capital | Dead | Yield about 1% a month on resting capital | $8–40/month at $2K | report withheld (internal) |
| Cross-venue UFC convergence | Buy the cheap venue, sell the dear one, wait for the gap to close | Dead | Gap converges, but spread cost at entry kills it | +0.5% gross, −3.7% net | report withheld (internal) |
| Coherence pairs | Arbitrage logically linked markets | Dead | Illiquid | — | |
| Fading behavioural biases | Take the other side of retail biases | Dead | Every counterparty turned out to be a maker | — | |
| Resolution-rule surprises | Trade markets whose rules text diverges from the headline | Dead | 195 surprises found; buying the mismatch class loses | 195 | report withheld (internal) |
| Supervised signal mining | Machine-learn a signal from the trade panel | Dead | Null | — | |

**Structural studies**

| Study | Idea | Verdict | Why it ended | Key number | Source |
|---|---|---|---|---|---|
| Copy-trading feasibility | Follow profitable Polymarket wallets | Dead | Seven variants killed under pre-registered kill rules | 7/7 failed | [`copy-trading/CLOSEOUT.md`](copy-trading/CLOSEOUT.md) |
| Order-flow reversal (Track B) | Volume-scaled order-flow imbalance predicts reversal in UFC markets | Closed — confirmed, sub-fee | Confirmed in a holdout registered before scoring; effect about a quarter of the fee | Q5−Q1 = −1.13¢, t = −6.29 | [`closures/CLOSURE_track_b_ofi.md`](closures/CLOSURE_track_b_ofi.md) |
| Kalshi macro drift (Track A) | Pre-release drift in macro contracts | Dead | Killed at six events, below kill rule (N < 20) | n=6 | report withheld (internal) |
| Market-structure analysis | — | — | Withheld — paper in preparation | — | — |
| Candidate A | — | Parked | Out-of-sample test pending; withheld | — | — |
| Candidate B | — | Parked | Out-of-sample test pending; withheld | — | — |

## Bugs that nearly fooled me

| Bug | How it showed up | Effect on the result | Source |
|---|---|---|---|
| Mirrored YES/NO complement legs in trade extracts | Bin prices summing to ~1.78 | ~1.39× trade duplication; the weather strategy's income fell ~20× once removed | [`weather-reports/leg_check.md`](weather-reports/leg_check.md), [`closures/CLOSURE_weather.md`](closures/CLOSURE_weather.md) |
| °C vs °F on London weather bins | Impossible bin assignments | Part of the same correction | [`closures/CLOSURE_weather.md`](closures/CLOSURE_weather.md) |
| Tail bins treated as bounded | Open-ended "above X" bins flagged as impossible | Removed the last positive cell in the weather study | [`closures/CLOSURE_weather.md`](closures/CLOSURE_weather.md) |
| Price lookups keyed on condition ID instead of asset ID | Phantom markouts | Holdout long-horizon markouts inflated to +12¢/c (was actually ~0) | report withheld (internal) |
| Entry price falling back to the first trade | Baskets "entered" on markets that did not yet exist | 93% of basket entries invalid; strategy dead | [`edge-reports/p2_bugcheck.md`](edge-reports/p2_bugcheck.md) |
| Regex matching prop markets as moneylines | Soccer sample contaminated with props | Sports test rerun on moneylines only | [`edge-reports/sports_fix.md`](edge-reports/sports_fix.md) |
| Survivorship in persistence tests | Filtering on ≥50 trades kept only wallets that kept trading | Apparent 30.5% persistence fell to no persistence for any group | report withheld (internal) |

## How I worked

- Kill rules and gates written before results; registered versus exploratory labelled as such.
- Fits end before evaluation windows; fresh-period holdouts are the only evidence I treat as proof.
- Hard assertions in the data layer stop the run instead of warning.
- With 25 strategies and 60 phase reports, false positives are expected by chance alone; several early "edges" in this ledger died exactly that way, which is why nothing here is called an edge until it survives data it has never seen.

## Repository structure

- `edge-reports/` — per-phase analysis reports (23)
- `weather-reports/` — weather-market study (6)
- `copy-trading/` — copy-trading feasibility study and its pre-registration (2)
- `closures/` — closure notes for completed sub-studies (4)
- `FINDINGS.md` — one row per strategy with verdict and source

## Data

Raw trade data (Polymarket on-chain and API, the Kalshi archive) is not distributed. Each report names the files it used; the pipelines rebuild them from public sources.
