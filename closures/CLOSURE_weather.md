# Weather Markets (Polymarket Daily-High) — closed 2026

**Question:** Do Polymarket daily-temperature markets misprice impossible
outcomes after METAR observations?

**What I expected:** Not recorded (exploratory).

**What I found:** Real edge, tiny, three bugs.

Corrected numbers (weather/report/steps_6_7_corrected.md):
- NYC median monthly: $62 at 100% capture, $15 at 25% capture.
- London median monthly: $129 at 100% capture, $32 at 25% capture.
- Combined at 25% capture with spread and loss adjustments: ~$44/month.
  (weather/report/steps_6_7_corrected.md line 85)

Phase 2 reported P&L was 20× inflated: NYC $28K → $1.3K; London $35K → $2.3K.

**Why I stopped:** Three bugs discovered in sequence:
1. Leg mirroring — eval_extract emits YES+NO token legs; double-counted.
2. °C/°F unit mismatch — London bin midpoints in °C, running_max in °F.
   (weather/report/leg_check.md line 59)
3. Upper-tail unbounded bins falsely flagged as "impossible."

After all three fixes, the edge is real but ~$44/month at 25% capture.
Holdout impossible: HuggingFace dataset ends March 2026, no live API path.

**What it rules out:** Weather market impossible-bin mispricing is not a viable
income source at current Polymarket volumes.

**What would reopen it:** Nothing — closed. Live METAR API access would enable
verification but the corrected edge size (~$44/month) does not justify the
infrastructure.
