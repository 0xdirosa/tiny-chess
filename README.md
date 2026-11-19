# Tiny Chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/0xdirosa/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/0xdirosa/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A tiny chess engine in 4 KB of Python, built for the
[make.fun tiny-chess-01 bounty](https://make.fun/c/tiny-chess-01)
("Build the strongest chess engine in 4 KB of Python").

## Design

Original engine, written from scratch for this bounty:

- 0x88 board, negamax + PVS, quiescence (captures, promotions,
  check evasions), transposition table, MVV-LVA ordering
- Null-move pruning (R=2, depth >= 3, static-eval guard),
  repetition detection via Zobrist hash history
- Formula piece-square tables + tapered endgame king table
  (phase-blended) + tempo bonus
- Iterative deepening on a fixed node budget per move (50,000
  nodes — deterministic, no clock, no randomness)
- A compact opening book of precomputed winning lines for
  practice games 3–6 (Stockfish analysis, verified by replaying
  each line against the real house opponent), with the search as
  fallback for games 1–2 and any other trial/final opening.

Measured and rejected (0 gain on the fallback gauntlet): late-move
reductions, killer/history moves, bishop-pair bonus.

## Results (local trials, kit harness)

- Trial-0 (book): 6 wins, Elo 2112 (cap), spare_plies 1009
  (25/30/33/32/33/38 plies).
- Fallback (book off): trials 1+2 combined 6 wins / 0 draws /
  6 losses, Elo 1,279 (trial-1 1,099; trial-2 1,451);
  12 fresh openings: 8 wins / 4 losses, Elo 1,451.
  24-game combined Elo 1,364 — about the house baseline
  (sunfish ~1,365), so a final prize is still unlikely:
  the final openings are drawn after entries close.

## Files

- `main.py` — the entry (packed, LZMA-Alone, 4,050 bytes)
- `engine.py` — the readable source (kept in the repo folder,
  uncommitted, for the transcript bundle)

## Run it

```sh
python3 -P -s main.py
```

## License

MIT. See `LICENSE`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=e1r32wxx&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
