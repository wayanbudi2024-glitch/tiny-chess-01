# tiny-chess-01 — a 4 KB chess engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/wayanbudi2024-glitch/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/wayanbudi2024-glitch/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in one `main.py` (Python 3.13, stdlib only, ≤4,096 bytes) for [make.fun's Tiny chess bounty](https://make.fun/bounty/tiny-chess-01).

## Engine

0x88 board, fixed-seed Zobrist, piece list. Negamax alpha-beta, iterative deepening, fixed **120,000 nodes/move**. Quiescence, TT, null-move, LMR, killer/history, MVV-LVA. Eval: material + 7 PSTs (tapered king) + bishop-pair +30; incremental eval; repetition 0. No model calls, no wall clock.

## Size

`main.py` 3,856 bytes (source `golfed2.py`, 11,223 bytes, `makefun compress`). PSTs from scratch, not sunfish's GPL tables.
