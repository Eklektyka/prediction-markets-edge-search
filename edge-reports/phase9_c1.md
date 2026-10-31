## C1 — Coherence Pairs

**Method:** Among resolved 'by <date>' markets, identify pairs (A,B) where A ⇒ B (same question stem, A deadline earlier than B). If event happens by A's deadline, it certainly happened by B's later deadline.
**LLM pass 1:** Automated stem extraction + deadline ordering → 2634 raw pairs from 129 stem groups.
**LLM pass 2:** Filter out non-monotonic questions ('exactly', 'between'), drop A=YES/B=NO wrong pairs → 2606 valid pairs.
**Selected:** 30 pairs across 30 distinct stems.
**Wrong pairs (A=YES, B=NO):** 0

### Side-Mapping Table

| # | Stem | Cat | A deadline | B deadline | Gap | A res | B res |
|---|------|-----|------------|------------|-----|-------|-------|
| 1 | another 70 or above earthquake | Tech | 2026-03-31 | 2026-04-30 | 30d | YES | YES |
| 2 | chatgpt outage | Tech | 2026-02-21 | 2026-02-28 | 7d | YES | YES |
| 3 | cilia flores released from custody | Poli | 2026-01-09 | 2026-01-31 | 22d | NO | NO |
| 4 | critical discord incident | Cult | 2026-03-31 | 2026-04-30 | 30d | YES | YES |
| 5 | cs2 market cap reaches 6 billion ag | Spor | 2025-12-31 | 2026-01-31 | 31d | NO | NO |
| 6 | dow hits 50k | Fina | 2026-01-16 | 2026-01-23 | 7d | NO | NO |
| 7 | gervonta davis arrested | Spor | 2026-01-23 | 2026-01-25 | 2d | NO | NO |
| 8 | gpt53 released | Tech | 2026-02-14 | 2026-02-21 | 7d | YES | YES |
| 9 | houthi strike on israel | Poli | 2026-03-06 | 2026-03-15 | 9d | NO | NO |
| 10 | internet access restored in iran | Poli | 2026-03-07 | 2026-03-14 | 7d | NO | NO |
| 11 | iran strike on qatar | Poli | 2026-01-31 | 2026-02-28 | 28d | NO | YES |
| 12 | israel strikes iran | Poli | 2026-01-09 | 2026-01-11 | 2d | NO | NO |
| 13 | israel x iran ceasefire broken | Poli | 2026-03-31 | 2026-06-30 | 91d | YES | YES |
| 14 | japanese snap election called | Poli | 2025-12-31 | 2026-01-31 | 31d | NO | YES |
| 15 | joseph aoun out as president of leb | Poli | 2025-10-31 | 2025-12-31 | 61d | NO | NO |
| 16 | khamenei public appearance | Poli | 2026-02-13 | 2026-02-20 | 7d | YES | YES |
| 17 | maduro out | Poli | 2026-01-31 | 2026-02-28 | 28d | YES | YES |
| 18 | markwayne mullin confirmed as dhs s | Poli | 2026-03-31 | 2026-04-30 | 30d | YES | YES |
| 19 | military action against iran ends | Poli | 2026-03-11 | 2026-03-13 | 2d | NO | NO |
| 20 | nicols maduro released from custody | Poli | 2026-01-09 | 2026-01-31 | 22d | NO | NO |
| 21 | pakistan strikes afghanistan | Poli | 2026-02-28 | 2026-03-31 | 31d | YES | YES |
| 22 | russia strike on kyiv municipality | Poli | 2026-01-10 | 2026-01-17 | 7d | YES | YES |
| 23 | russia x ukraine ceasefire | Poli | 2026-01-31 | 2026-02-28 | 28d | NO | NO |
| 24 | russia x ukraine prisoner exchange | Poli | 2026-02-14 | 2026-02-28 | 14d | YES | YES |
| 25 | thailand x cambodia ceasefire | Poli | 2026-01-31 | 2026-03-31 | 59d | YES | YES |
| 26 | thailand x cambodia military engage | Poli | 2025-11-30 | 2025-12-31 | 31d | YES | YES |
| 27 | total internet blackout in iran | Poli | 2026-02-28 | 2026-03-31 | 31d | YES | YES |
| 28 | trump announces a new drug boat str | Poli | 2026-02-14 | 2026-02-21 | 7d | YES | YES |
| 29 | trump announces new drug boat strik | Poli | 2026-01-05 | 2026-01-10 | 5d | NO | NO |
| 30 | trump cabinet member out | Poli | 2025-12-31 | 2026-03-31 | 90d | NO | YES |

### Daily Price Gap (price_A − price_B)

A positive gap = violation (earlier-deadline market priced higher than later-deadline).

| # | Stem | Days | Mean gap | Max gap | >2c% | >4c% | Max run |
|---|------|------|----------|---------|------|------|---------|
| 2 | chatgpt outage | 7 | -15.00c | 0.00c | 0% | 0% | 0d |
| 3 | cilia flores released from cus | 2 | -1.95c | -1.70c | 0% | 0% | 0d |
| 9 | houthi strike on israel | 2 | -19.00c | -18.00c | 0% | 0% | 0d |
| 10 | internet access restored in ir | 1 | -3.60c | -3.60c | 0% | 0% | 0d |
| 11 | iran strike on qatar | 1 | -10.40c | -10.40c | 0% | 0% | 0d |
| 13 | israel x iran ceasefire broken | 96 | -4.71c | 30.90c | 31% | 31% | 30d |
| 17 | maduro out | 42 | -3.62c | 0.00c | 0% | 0% | 0d |
| 18 | markwayne mullin confirmed as  | 23 | -19.00c | -8.00c | 0% | 0% | 0d |
| 20 | nicols maduro released from cu | 4 | -1.92c | -1.60c | 0% | 0% | 0d |
| 22 | russia strike on kyiv municipa | 7 | -3.70c | 6.70c | 29% | 14% | 1d |
| 23 | russia x ukraine ceasefire | 7 | -5.93c | -4.90c | 0% | 0% | 0d |
| 24 | russia x ukraine prisoner exch | 9 | -0.57c | 0.00c | 0% | 0% | 0d |
| 25 | thailand x cambodia ceasefire | 36 | +4.06c | 4.70c | 94% | 94% | 34d |
| 28 | trump announces a new drug boa | 3 | +0.00c | 0.00c | 0% | 0% | 0d |

**Aggregate:** mean gap across pairs -6.10c, mean share of days >2c: 11%

### Strategy P&L (buy B / sell A at first >2c violation, hold to resolution)

**Pairs with fills:** 1/14
**Mean P&L/contract (net):** -12.90c
**Median P&L/contract (net):** -12.90c
**95% CI:** [-12.90c, -12.90c]
**Positive:** 0/1

### T1–T5 Tradeability

**T1 (CI>0):** FAIL — [-12.90c, -12.90c]
**T2 (≥60% months >0):** FAIL — 0/1
**T3:** Concentration inherent with 30 pairs — assessment deferred
**T4 (latency):** PASS — fill_after based
**T5:** Insufficient signal frequency for monthly income estimate

**Verdict: NOT TRADEABLE**
