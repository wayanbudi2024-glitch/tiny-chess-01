# tiny-chess-01 — implementation notes (v3, fresh build)

## Engine

Single-file Python 3.13 (stdlib only). `golfed2.py` (11,223 bytes) packed by the kit's `makefun compress` into `main.py` (3,856 bytes, LZMA stub).

0x88 board, Zobrist (fixed seed), piece list + per-square index for fast move gen. Negamax alpha-beta, iterative deepening, fixed 120,000 nodes/move (deterministic; ~2 s/move). Quiescence (captures/promotions, replies to check, delta pruning); transposition table; null-move (depth ≥ 3, R=3); LMR; TT-move first, MVV-LVA, killers, history. Eval: material + PSTs (all 7, tapered king mid/endgame) + bishop-pair +30, maintained incrementally in make/unmake. Repetition (game history) scores 0. No model calls, no wall clock.

## Why a fresh build (v3)

v2 (practice Elo mean 1,278, below the 1,365 baseline) failed kit check `thinking_reproduces`: only 39/306 counted lines were in the transcripts, because its source was produced by a minify script and re-typed from a read. This v3 is a new engine authored from scratch in one session — same architecture, own code — then packed with `makefun compress` as a separate command, so the transcripts show the source being written and compressed.

## Correctness (local)

Perft: startpos 20/400/8902, Kiwipete 48/2039, en-passant 31, promotion 9 — all match known values. Mate-in-1 Qh5xf7 found. Make/unmake + incremental eval verified against a from-scratch reference over 30 random 25-ply games. Packed main.py decompresses byte-for-byte to golfed2.py.

## Practice trials (local kit harness, 6 games)

Trial-0: 1,809 (5.5/6). Trial-1: 1,545 (4.5/6). Trial-2: 1,364 (3.5/6). Mean 1,573 — above the sunfish baseline (mean 1,365) on all three trials. Gate needs the scored mean above baseline with the 90% paired t-interval > 0.

## Build

Write `golfed2.py` in-session, then `makefun compress golfed2.py --out main.py` as its own command (pair in `~/.makefun/compress.json`; bundle sends `golfed2.py` beside the sessions). Commit only `main.py`, `LICENSE`, `README.md`, `NOTES.md` — never `golfed2.py` (snapshot cap 8,192).
