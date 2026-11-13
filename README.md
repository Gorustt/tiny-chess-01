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

`main.py` is a zlib-compressed payload stored as an ASCII base85 string
literal, unpacked by a two-line loader (`import zlib as z,base64 as b` /
`exec(z.decompress(b.b85decode('...')))`). The rules allow a compressed
payload + unpacker, counted toward the 4,096-byte cap. Base85 (not latin-1)
keeps the file strict UTF-8 so the transcript gate can attribute its lines.
The readable source is `golf6.py` (kept out of the submitted repo; see
BUILD_LOG.md in the build workspace).

## Results (local practice trials, kit env — base85 build)

| Trial | Elo | W/D/L | Wall |
|---|---|---|---|
| 0 | 1,464 | 3/3/2 | 1,159 s |
| 1 | 1,326 | 3/1/4 | 1,658 s |
| 2 | 1,326 | 3/1/4 | 1,099 s |

Mean Elo **1,372** — exactly the v6 mean, above the sunfish 4K baseline (~1,325) and the 1,154 entry floor. (The latin-1 build's mean was 1,348; base85 trial 1 recovered the time-loss game.) Trial 1's 1,658 s wall is the closest to the 1,800 s limit — a watch item, but no move timed out. Perft 16/16 on the 5 standard positions. Determinism verified: two bench runs byte-identical (best moves + node counts). No model calls, stdlib only.
