# tiny-chess-01 entry — 4 KB chess engine

A complete chess engine in a single `main.py` of **3,685 bytes** (limit 4,096),
Python 3.13 standard library only, deterministic. Built for the make.fun
tiny-chess-01 bounty: the strongest chess engine in 4 KB of Python.

## How it plays

- Sunfish-style 0x88 board, negamax alpha-beta with quiescence search
  (captures/promotions; all evasions when in check)
- Iterative deepening to a fixed global budget of **100,000 nodes/move**
  (deterministic — the same position always yields the same move)
- Null-move pruning (R=3), transposition table, persistent killer moves,
  MVV-LVA + TT-move ordering
- Evaluation: material + Michniewski piece-square tables (7 tables packed
  as 32-character strings, expanded at startup) + bishop-pair bonus

`main.py` is a one-line loader: the engine source is lzma-compressed
(FORMAT_ALONE, preset 9) and base85-encoded. The readable development
source is in `dev/engine5.py`; `dev/build_main.py` builds `main.py` from it.
See `BUILD_NOTES.md` for the full build and test history.

## Local practice results

Three practice trials against the kit's 8 house engines (710–2,112 Elo),
run with the kit's local env server + scorer (no Docker):

| Trial | Elo | W–D–L |
|---|---|---|
| 0 | 1,326 | 2–3–3 |
| 1 | 1,396 | 3–2–3 |
| 2 | 1,748 | 5–3–0 |
| **Mean** | **1,490** | **10–8–6** |

Mean 1,490 Elo is **165 above the kit's Sunfish baseline (1,325)** and
clears the 1,154 practice gate. Trial 2 is likely a high outlier
(8-game Elo estimates are noisy); the mean of trials 0–1 is 1,361, still
above baseline. True strength is estimated at ~1,350–1,450 Elo.

Revision (late move reduction): after the trials above, LMR was added
to the search (quiet moves after the first 3, at depth > 2, first searched
at reduced depth with a null window). Re-running trial-0 with the revised
engine scored **1,464 Elo (2–5–1)**, up from 1,326 — no regression,
losses 3→1.
Slowest move across 1,253 moves in the three trials: 4,143 ms
(10 s limit). Perft depth 1–3 on 9 positions verified against
python-chess.

## License

MIT — see `LICENSE`.
