# tiny-chess-01

A 4 KB Python chess engine for the make.fun tiny-chess-01 bounty.

## Engine

- 0x88 board, full legal move generation (castling, en passant, promotion)
- Negamax + alpha-beta with PVS, quiescence (captures/promotions + check evasions)
- Transposition table (Zobrist, EXACT/LOWER/UPPER), killer + history ordering
- Piece-square-table evaluation (midgame + endgame king tables, tapered by phase)
- Null-move pruning, conservative LMR, iterative deepening to a fixed 100,000-node budget per move
- Deterministic: fixed node budget, no wall clock, no randomness (seeded Zobrist)

## Format

`main.py` is an lzma-compressed payload stored as a latin-1 string literal
(with null/CR bytes escaped). The rules allow a compressed payload + unpacker,
counted toward the 4,096-byte cap. The readable source is `golf4.py`
(kept out of the submitted repo; see BUILD_LOG.md in the build workspace).

## Results (local practice trials, kit env)

| Trial | Elo | W/D/L | Wall |
|---|---|---|---|
| 0 | 1,464 | 3/3/2 | 955 s |
| 1 | 1,253 | 2/2/4 | 1,486 s |
| 2 | 1,326 | 3/1/4 | 1,237 s |

Mean Elo **1,348** — above the sunfish 4K baseline (~1,325) and the 1,154 entry floor.
Perft 16/16 on the 5 standard positions. No model calls, stdlib only.
