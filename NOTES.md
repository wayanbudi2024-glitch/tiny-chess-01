# tiny-chess-01 — implementation notes (v2)

## The engine

Single-file Python 3.13 (stdlib only). `golfed2.py` (8,097 bytes) packed by the kit's `makefun compress` into `main.py` (3,415 bytes, LZMA + latin-1 stub).

0x88 board, Zobrist (fixed seed). Negamax alpha-beta, iterative deepening, fixed 120,000 nodes/move (deterministic; ~2.2 s/move; 6-game trial ~610-750 s). Quiescence over captures/promotions and replies to check; transposition table; null-move (depth ≥ 3, R=3); LMR; TT-move first, MVV-LVA, killers, history. Eval: material + Michniewski PSTs (all 7, tapered king mid/endgame) + bishop-pair +30. Repetition (game history and in-search path) scores 0. No model calls or wall clock.

## Correctness (local, kit harness + python-chess)

Perft: startpos d1–d3, Kiwipete, en-passant/promotion positions — all match python-chess. Mate-in-1 Qh5xf7 found. Zobrist hash and make/unmake verified. Compressed build decompresses to golfed2.py byte-for-byte; perft + mate-in-1 re-run OK.

## Practice trials (local kit harness, 6 games)

| Build | Trial-0 | Trial-1 | Trial-2 | Mean |
|---|---|---|---|---|
| v2 (this entry) | 1,191 | 1,451 | 1,191 | **1,278** |
| v1 (no king eval/bishop pair) | 1,279 | 1,099 | 1,279 | 1,219 |
| sunfish 4k baseline | 1,364 | 1,451 | 1,279 | 1,365 |

## Honest assessment

v2 improves on v1 (+59 Elo mean) but stays below the sunfish baseline (1,278 vs 1,365; below on trial-0 and trial-2, tied on trial-1). The prize gate needs the scored mean above the baseline with the 90% paired t-interval > 0, so on current evidence this entry is unlikely to win. Trial-2 drew all 3 draws by threefold repetition; those look forced, not a bug. The v1 NOTES claim of a 1,654/2,112 "full engine" could not be reproduced — that source was lost in a /tmp wipe, so treat those numbers as unconfirmed.

## Build

Source: `~/workspace/tinychess-dev/eng2.py` → `minify2.py` → `golfed2.py` → `makefun compress golfed2.py --out main.py` (3,415 bytes). Pair in `~/.makefun/compress.json`; bundle sends `golfed2.py` beside the sessions. Do NOT commit `golfed2.py` (breaks the 8,192-byte snapshot cap); commit only `main.py`, `LICENSE`, `README.md`, `NOTES.md`.
