# tiny-chess-01 — a 4 KB chess engine

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)

An entry for [make.fun's "Tiny chess" bounty](https://make.fun/bounty/tiny-chess-01): a complete chess engine in a single `main.py` of at most 4,096 bytes.

## The engine

`main.py` (Python 3.13, stdlib only) plays through the env (`POST /game`, `/move`, `/resign`): **0x88 board** with Zobrist hashing (fixed seed, deterministic); **negamax alpha-beta** with iterative deepening at a fixed **120,000 nodes/move** (~2.2 s/move); **quiescence** (captures, promotions, replies to check), **transposition table**, **null-move pruning**, **late-move reductions**, killer + history ordering; **eval** of material + Michniewski piece-square tables (all 7, incl. tapered king middlegame/endgame) + bishop-pair bonus (+30); repetition scored 0. No model calls, no network except to the env, no wall clock.

## Size

`main.py` is 3,415 bytes (source `golfed2.py`, 8,097 bytes, packed with `makefun compress`). Snapshot under 8,192 bytes. PSTs are Michniewski's (MIT-compatible), not sunfish's GPL tables.

## Practice results (local kit harness, 6 games)

| Trial | Result | Elo |
|---|---|---|
| trial-0 | 2W 1D 3L | 1,191 |
| trial-1 | 4W 0D 2L | 1,451 |
| trial-2 | 1W 3D 2L | 1,191 |
| **mean** | | **1,278** |

Baseline sunfish 4k: 1,364 / 1,451 / 1,279 (mean 1,365). Below baseline on practice; see NOTES.md.
