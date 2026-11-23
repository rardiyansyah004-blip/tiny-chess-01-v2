# tiny-chess-01-v2

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/rardiyansyah004-blip/tiny-chess-01-v2/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/rardiyansyah004-blip/tiny-chess-01-v2/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A chess engine in 4 KB of Python, built for the [make.fun](https://make.fun/bounty/tiny-chess-01) bounty "Build the strongest chess engine in 4 KB of Python". This is v2, a revision of the v1 engine (practice Elo 1279) chasing a payable position.

`main.py` is the packed entry (4,076 bytes, at most 4,096), produced by the kit's `compress` from `engine2.py`, the readable source kept beside it in this folder (uncommitted — it is 10,407 bytes, over the 4,096 per-file cap, and is sent as the pack source at bundle time).

## Engine (own work, not a sunfish fork)

0x88 board, principal-variation search (PVS) with late-move reductions (LMR) and iterative deepening under a FIXED node budget (120,000 nodes/move — no clock, no unseeded randomness), quiescence over captures/promotions/check replies with delta pruning, transposition table, killer moves, MVV-LVA ordering, tapered PeSTO evaluation (midgame/endgame piece-square tables + piece values, phased by material, with separate endgame pawn and king tables), null-move pruning, and repetition avoidance replayed from the game's history. Deterministic: every move derives from the position, the game history and the fixed node budget.

## What changed vs v1

- PVS (null-window search + re-search) instead of plain alpha-beta.
- LMR: late quiet moves searched at reduced depth, re-searched on fail-high.
- Tapered PeSTO evaluation replaces the single-table eval: separate midgame/endgame values and tables, phased by remaining material.
- Separate endgame pawn and king tables (passed-pawn push, king centralisation).
- Killer-move ordering (history heuristic dropped to fit the 4 KB cap).
- Same 120,000-node budget, so per-move time stays ~1.5–3 s, well inside the 10 s move limit.

## Local practice results

Kit env server + scorer, practice/trial-0 (6 games vs the house gauntlet). See the handoff for the v2 number.

## Files

- `main.py` — the packed entry (4,076 bytes, at most 4,096)
- `engine2.py` — the readable source `main.py` was packed from (uncommitted; sent as pack source)
- `LICENSE` — MIT
