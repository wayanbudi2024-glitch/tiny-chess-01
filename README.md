# tiny-chess-01 — a 4 KB chess engine

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)

Entry for [make.fun's "Tiny chess" bounty](https://make.fun/bounty/tiny-chess-01): a complete chess engine in one `main.py` (Python 3.13, stdlib only), at most 4,096 bytes.

## Engine

0x88 board, Zobrist hashing (fixed seed, deterministic), piece list for fast move gen. Negamax alpha-beta, iterative deepening, fixed **120,000 nodes/move** (~2 s/move). Quiescence (captures, promotions, replies to check, delta pruning), transposition table, null-move pruning, LMR, killer + history, MVV-LVA. Eval: material + piece-square tables (all 7, tapered king middlegame/endgame) + bishop-pair +30; incremental eval; repetition (game history) scores 0. No model calls, no wall clock.

## Size

`main.py` is 3,856 bytes (readable source `golfed2.py`, 11,223 bytes, packed with `makefun compress`). Snapshot under 8,192 bytes. PSTs are standard tables written from scratch, not sunfish's GPL tables.

## Practice

Local kit harness, 6 games per trial — see NOTES.md.
