## P1 Fix — CLOB Snapshots on D-Map Markets

**Problem:** The 30 D-map markets are all resolved (closed Jul 2025–Mar 2026).
Their CLOB books no longer exist — cannot snapshot them. The only active politics
markets with CLOB data are 2028 presidential markets, which have 100–1000× more
depth (5.2M contracts median) than short-horizon events had (~50K daily volume).

**Implication:** The snapshot-implied fill share (0.0005%) is from the wrong market
class. For short-horizon politics markets (primaries, referenda), typical depth was
likely 1–2 orders of magnitude lower than 2028 presidential. A proper measurement
requires snapshotting active short-horizon events during their trading window.

**Action:** Cannot recompute until new short-horizon politics events are active
and snapshotted. The spread estimates (Roll 1.1c, Direct 0.9c) from trade data
are valid; only the fill-share proxy is unreliable.

**Status:** BLOCKED on market availability.
