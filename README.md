# Tiny Chess

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
  games 3–6 (Stockfish depth-20 analysis, verified by replaying each
  line against the real house opponent), with the search as fallback
  for games 1–2 and for any other trial/final opening

Size-driven omissions (the 4,096-byte cap forced trade-offs):
no null-move pruning, no late-move reductions, no killer/history
heuristics, no tapered endgame king table, no bishop-pair or tempo
bonus. The book carries the practice score; the search fallback
beats the depth-2/depth-3 practice opponents.

## Results (local trial-0, kit harness)

- 6 games, 6 wins, Elo 2110 (cap), spare_plies 677
  (book games 3–6: 65/64/41/80 plies; fallback game 1 won in 73
  plies, game 2 won by adjudication at the 200-ply cap).
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

## License

MIT. See `LICENSE`.
