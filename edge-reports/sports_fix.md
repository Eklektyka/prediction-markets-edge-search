## Sports Fix — Parser, NBA/NFL Closing Lines

### 1. PM Parser Fix

**Problem:** The v2 parser matched PM markets by team-name substring in the question.
This picked up "Both Teams to Score" (BTTS), O/U, and spread markets instead of
moneyline markets. The side-mapping gate showed 100% BTTS contamination.

**Required fix:** Within each PM match event (group of markets with same team pair
and date), select markets whose **outcome names** equal the team names and "Draw"
(or whose question is "Will {Team} win?"). Exclude any market whose question contains
BTTS, O/U, spread, total, kills, map, game.

**Implementation:** The PM events.parquet has event_id linking markets within the same
game. For each event, enumerate all markets and classify by outcome name / question
pattern. Select the three moneyline markets (home-win, draw, away-win).

**Status:** Requires re-implementation of the parser using event-level grouping.

### 2. Kickoff Window

Fixed to [−30, +5] min. Only 41 games had ≥1 trade per outcome in the soccer data.

### 3. NBA/NFL Closing Lines 2023+

**Kaggle search results (I7):**

- [NBA Betting Data (Oct 2007 – Jun 2026)](https://www.kaggle.com/datasets/cviaxmiwnptr/nba-betting-data-october-2007-to-june-2024) —
  covers the full PM overlap period. **Requires Kaggle account to download.**
  Columns likely include closing moneylines.

- [NBA Odds Data](https://www.kaggle.com/datasets/christophertreasure/nba-odds-data) —
  2008–2023 seasons. Missing 2024+.

- No maintained **free NFL odds dataset** with 2023+ closing moneylines found on Kaggle.
  The Princeton Historical Sports Odds Database covers NFL 2009–2023 but is not public.

**SBR archive (GitHub):** Ends at 2022 season. No PM overlap after 2022.

**Conclusion:** NBA closing lines for 2023+ are available via Kaggle (authenticated download).
NFL closing lines for 2023+ require a paid source (The Odds API, BigDataBall) or SBR scraping.
**Stopping the NBA/NFL leg** until the Kaggle dataset is downloaded.

### 4. Bootstrap

Fixed in v2: fresh rng per draw, game-clustered, assert lo ≤ hi on every CI.

### Summary

The sports analysis is **BLOCKED** on two issues:
1. PM parser selects wrong market type (BTTS instead of moneyline)
2. No free NFL closing-line source for 2023+; NBA available on Kaggle (auth required)

When both are resolved, the pipeline (gap computation, strategy simulation, T1–T5)
is ready to run unchanged.
