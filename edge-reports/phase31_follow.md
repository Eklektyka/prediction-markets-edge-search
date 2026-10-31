# Phase 31 — Following the Sharps' First Fill

_Generated: 2026-09-20. Corrected methodology (phase31_follow.py)._
_Cluster-0 wallets: 10,137. Pre-game sports only (ts < close_at)._
_Fit: Jul–Sep 2025 (18,348 signals). Eval: Oct–Dec 2025 (39,262 signals)._

## 1. Signals

**Definition:** First cluster-0 fill on a given side of an asset, ≥50
contracts, no cluster-0 fill on that side in the prior 30 min, pre-game
only.

**Total signals:** 74,286 (fit 18,348 / eval 39,262)
**Signals/day:** 347

**By sport:**

| Sport | Signals | Share |
|-------|---------|-------|
| Other | 22,262 | 30% |
| Basketball | 16,087 | 22% |
| American Football | 8,406 | 11% |
| Baseball | 8,266 | 11% |
| Soccer | 7,652 | 10% |
| Esports | 6,768 | 9% |
| Tennis | 3,124 | 4% |
| Motorsport | 755 | 1% |
| Golf | 421 | 1% |
| Boxing/Cricket/Hockey/MMA | 545 | 1% |

**By hours to kickoff:**

| Bucket | Signals | Share |
|--------|---------|-------|
| 0–2 h | 2,415 | 3% |
| 2–6 h | 25,756 | 35% |
| 6–12 h | 15,087 | 20% |
| 12–24 h | 12,055 | 16% |
| 24 h+ | 18,967 | 26% |

Most signals fire 2–12 h before kickoff — these are position-building
trades by profitable wallets, not reactions to sudden line moves.

## 2. Move Anatomy

The central question: after a cluster-0 wallet fills, does the PM price
move in the signal's direction?

**Mean move (signal direction, +5 min):** −0.02 c
**Median move:** +0.00 c
**Positive-direction share:** 21%

The signal has **no predictive power**. Only 21% of signals are followed
by a same-direction move — worse than a coin flip. The median move is
exactly zero: the price does not move at all.

| Latency | Residual (P25 / median / P75) | Capacity (median) |
|---------|-------------------------------|-------------------|
| 2 s | 0.00 / 0.00 / 0.00 | 180 contracts |
| 5 s | 0.00 / 0.00 / 0.00 | 183 |
| 10 s | 0.00 / 0.00 / 0.00 | 194 |
| 30 s | 0.00 / 0.00 / 0.00 | 203 |
| 60 s | 0.00 / 0.00 / 0.00 | 230 |

**Residual is zero at every latency, including 2 s.** There is no move
to capture. This is not a speed problem — the signal itself is empty.

**By sport (mean move, median residual at 5 s):**

| Sport | N | Mean move | Res@5s |
|-------|---|----------|--------|
| Baseball | 8,266 | +0.25 c | 0.00 |
| Motorsport | 755 | +0.28 c | 0.00 |
| Esports | 6,768 | +0.04 c | 0.00 |
| Soccer | 7,652 | +0.05 c | 0.00 |
| Other | 22,262 | −0.02 c | 0.00 |
| American Football | 8,406 | −0.13 c | 0.00 |
| Basketball | 16,087 | −0.16 c | 0.00 |

No sport shows a usable residual. Baseball and Motorsport have slightly
positive mean moves but zero residual — the moves that do occur are
already consumed.

**By liquidity tercile:**

| Tercile | N | Mean move | Res@5s |
|---------|---|----------|--------|
| Low | 1,645 | +0.40 c | 0.00 |
| Mid | 11,580 | +0.24 c | 0.00 |
| High | 61,061 | −0.08 c | 0.00 |

Low-liquidity markets show a slightly positive mean move (+0.40 c) but
zero residual. The positive mean is absorbed by wider bid-ask spreads
and lower fill rates.

## 3. Strategy (€2,000 capital)

**Parameters:** €200 / signal, max 5 concurrent positions, €2,000 total
capital. Exit at +30 min or when gap to +5 min price < 1 c. Taker fee:
2% × P(1−P) per side.

### Unbounded results (unlimited capital recycling)

| Latency | Period | Fills | FM% | Mean/trade | CI lo | CI hi |
|---------|--------|-------|-----|-----------|-------|-------|
| 2 s | fit | 5,094 | 100% | −€3.28 | −€5.37 | −€0.93 |
| 2 s | eval | 11,648 | 100% | −€9.25 | −€11.93 | −€7.05 |
| 5 s | fit | 4,736 | 86% | −€4.08 | −€6.22 | −€1.90 |
| 5 s | eval | 11,086 | 86% | −€9.04 | −€11.32 | −€7.14 |
| 10 s | fit | 4,422 | 79% | −€4.46 | −€6.35 | −€2.29 |
| 10 s | eval | 10,549 | 77% | −€9.15 | −€11.52 | −€7.21 |

CIs are market-clustered bootstrap (by event). Every period × latency
combination has a CI entirely below zero. No month is positive (0/6).

### Capital-bounded result

The unbounded simulation processes 11,086 fills on €2,000 of capital.
In reality, losses deplete the pool. At −€9.04 mean per trade on €200
positions (−4.5% per trade), the capital pool is exhausted after ~221
trades — **6% of the month's signals**. The strategy wipes out the full
€2,000 within the first few days of each eval month.

| Metric | 5 s latency, eval |
|--------|-------------------|
| Mean P&L per trade | −€9.04 |
| Trades to capital exhaustion | ~221 |
| Days to wipeout (at signal rate) | ~2–3 |
| **Capital-bounded loss/month** | **−€2,000 (total wipeout)** |
| Months positive | 0 / 3 |

### First-mover flag

At 5 s latency, 86% of fills are first-mover (no same-side taker fill
between signal time and signal + 5 s). This means you would typically be
the first follower — but being first to a worthless signal is not an
advantage.

### T1–T5 (NOT TRADEABLE, 1/5)

- T1 eval CI > 0: **FAIL** — CI [−€11.32, −€7.14]
- T2 ≥ 60% months > 0: **FAIL** — 0/3
- T3 concentration: **FAIL** — ex-best-month deeply negative
- T4 latency ≥ median reaction: **PASS**
- T5 ≥ €100/month net: **FAIL** — −€2,000/month (capital-bounded)

## 4. Signal Quality

Precision = share of signals followed by a ≥ 2 c same-direction move
within 5 min.

| Period | Precision | N |
|--------|-----------|---|
| Fit (Jul–Sep) | 10.9% | 18,348 |
| Eval (Oct–Dec) | 6.6% | 39,262 |

Precision degrades from fit to eval, consistent with the cluster-0 list
going stale (built on Jul–Sep data, applied to Oct–Dec).

**By sport (fit + eval combined):**

| Sport | N | Precision |
|-------|---|-----------|
| Esports | 4,265 | 13.3% |
| Baseball | 8,123 | 13.1% |
| Tennis | 2,013 | 10.4% |
| Golf | 419 | 10.0% |
| Motorsport | 726 | 8.4% |
| Other | 18,504 | 7.8% |
| American Football | 6,877 | 5.3% |
| Basketball | 10,744 | 5.2% |
| Soccer | 5,505 | 4.4% |

Esports and Baseball have the highest precision (~13%), but even these
translate to zero residual at 5 s.

**Top-100 wallet filter (ranked on fit precision, ≥ 5 fit signals):**

| Metric | Top-100 (eval) | All (eval) |
|--------|---------------|------------|
| Precision | 19.3% | 6.6% |
| Mean move | +0.45 c | −0.12 c |
| Strategy P&L (5 s, unbounded) | −€823/month | −€33,619/month |
| Signals/month | 148 | 3,695 |
| Trades to wipeout | ~360 | ~221 |
| Capital-bounded loss/month | −€823 | −€2,000 |

Restricting to the 100 most precise fit-period wallets raises eval
precision to 19.3% and makes the mean move positive (+0.45 c). But the
strategy is still negative: −€823/month (capital-bounded). The positive
mean move is consumed by fees and adverse fills.

## 5. Address Persistence

| Metric | Value |
|--------|-------|
| Signal wallets active ≥ 3 months | 1,799 / 7,126 (25%) |
| Their share of signal volume | 62% |

**Fit-wallet survival into eval:**

| Month | Fit wallets still active | Share |
|-------|------------------------|-------|
| Oct 2025 | 910 / 2,033 | 45% |
| Nov 2025 | 696 / 2,033 | 34% |
| Dec 2025 | 577 / 2,033 | 28% |

The cluster-0 list decays: by December, only 28% of Jul–Sep wallets are
still generating signals. This is consistent with the precision drop
(10.9% → 6.6%) and means a static wallet list goes stale within one
quarter. A monthly rebuild would be needed, but the signal is worthless
regardless.

## 6. Followers Already Present

Non-cluster-0 taker fills on the signal side after each signal:

| Window | Fills | Volume | Distinct wallets | Markout (mean) | Markout (median) |
|--------|-------|--------|------------------|---------------|-----------------|
| 2 s | 4,846 | 2.6 M | 2,115 | +1.51 c | 0.00 c |
| 5 s | 7,023 | 3.6 M | 2,832 | +1.43 c | 0.00 c |
| 10 s | 12,766 | 6.3 M | 4,801 | +1.04 c | 0.00 c |
| 30 s | 27,459 | 12.2 M | 9,273 | +0.81 c | 0.00 c |

Follower activity is heavy: 12,766 non-cluster-0 fills within 10 s of
each signal from 4,801 distinct wallets. These followers have a positive
mean markout (+1.04 c at 10 s) but zero median — the distribution is
highly skewed, with a few large winners pulling the mean up. The
followers are not systematically profiting from the signal.

The follower volume (6.3 M contracts within 10 s) dwarfs the capacity
available — even if the signal worked, the liquidity would be gone.

## 7. Verdict

**The cluster-0 first-fill signal does not predict short-term PM price
moves.** The median +5 min move in the signal's direction is zero.
Residual is zero at every latency from 2 s to 60 s. This is not a
latency problem — there is nothing to be fast for.

**Why the signal is empty:** Cluster-0 wallets are profitable sports
bettors (Phase 26: +10.1 c resolution markout, ROC 64%/month). They are
informed about *outcomes* (who will win), not about *short-term price
dynamics*. Their first fill reflects a position-building decision hours
before kickoff. The PM price does not move when they fill because (a) the
information they trade on (sportsbook lines, injury news) has already
been partially priced in, and (b) their individual fill is too small
relative to market depth to move the price mechanically.

Phase 29 showed that PM *moves* complete within 60 s and 82% of capacity
is consumed in that window. But those moves are triggered by events (line
changes, news) that many participants observe simultaneously — the first
cluster-0 fill is not the trigger, it is one of many reactions.

**Capital-bounded result on €2,000 at 5 s latency (eval):**

| Metric | Value |
|--------|-------|
| Mean P&L per trade | −€9.04 |
| Trades to wipeout | ~221 |
| Monthly loss (capital-bounded) | −€2,000 (wipeout) |
| Months positive | 0 / 3 |
| **T1–T5** | **NOT TRADEABLE (1/5)** |

Even the best sub-filter (top-100 wallets by fit precision) loses
€823/month — the signal is not there.
