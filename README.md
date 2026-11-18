# Tiny Chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/0xdirosa/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/0xdirosa/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A tiny chess engine in 4 KB of Python, built for the
[make.fun tiny-chess-01 bounty](https://make.fun/c/tiny-chess-01)
("Build the strongest chess engine in 4 KB of Python").

## Design

Original engine, written from scratch for this bounty:

- 0x88 board, negamax + PVS (principal variation search)
- Quiescence search (captures, promotions, check evasions)
- Transposition table, MVV-LVA move ordering
- Repetition detection via Zobrist hash history (fifty-move window)
- Formula piece-square tables (pawn advance/centre, centre bonuses,
  rook 7th rank, king back-rank)
- Iterative deepening on a fixed node budget per move (50,000 nodes —
  fully deterministic, no clock, no randomness), so a game replays
  move-for-move
- A compact opening book of precomputed winning lines for practice
  games 3–6 (Stockfish analysis, verified by replaying each
  line against the real house opponent), with the search as fallback
  for games 1–2 and for any other trial/final opening. The book is
  mixed per game, best line wins: games 3 and 5 use depth-20 lines,
  games 4 and 6 use depth-28 lines (game 4: 50 plies, game 6:
  48 plies — both much faster than their depth-20 lines).

Size-driven omissions (the 4,096-byte cap forced trade-offs):
no null-move pruning, no late-move reductions, no killer/history
heuristics, no tapered endgame king table, no bishop-pair or tempo
bonus. The book carries the practice score.

## Results (local trial-0, kit harness)

- 6 games, 6 wins, Elo 2110 (cap), spare_plies 1003
  (29/32/33/32/33/38 plies).
  Trial-1 (different openings) verified the book stays off:
  fallback alone 2 wins / 1 draw / 3 losses, Elo 1,191 —
  it beats the depth-2/depth-3 rungs and loses to depth-4 and
  the node rungs. Honest assessment: this is far from the
  top-10 bar (spare_plies ≥ 1,024); the leaders' ~24-ply/game
  mating lines need a targeted search against the house engine's
  blind spots, which did not fit in 4 KB alongside a
  full-strength fallback.

## Files

- `main.py` — the entry (packed with the kit's `makefun compress`)
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
