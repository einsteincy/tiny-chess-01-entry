# tiny-chess-01-entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/einsteincy/tiny-chess-01-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/einsteincy/tiny-chess-01-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in a single `main.py` of **4,040 bytes** (limit 4,096), Python 3.13 standard library only, deterministic. Built for the [make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01): the strongest chess engine in 4 KB of Python, $500 in USDC.

## How it plays (engine11)

- Sunfish-style 0x88 board, negamax alpha-beta with quiescence search (captures/promotions; all evasions when in check)
- Principal Variation Search (PVS) + aspiration windows, iterative deepening to a fixed global budget of **170,000 nodes/move** (deterministic — the same position always yields the same move)
- Mate-distance pruning + mate-score adjustment in the transposition table (prefers the fastest mate), quiescence futility pruning, null-move pruning (R=3), transposition table, persistent killer moves, MVV-LVA + TT-move ordering
- Evaluation: PeSTO tapered piece-square tables (12 tables, middlegame + endgame) + bishop-pair bonus

`main.py` is a one-line loader: the engine source is lzma-compressed and run with exec, packed with the kit's `makefun compress` command.

## Run it

```sh
python3 main.py
```

## Local practice results

Three practice trials against the kit's 8 house engines (710–2,112 Elo), run with the kit's local env server + scorer (no Docker):

| Trial | Elo | W–D–L | Spare |
|---|---|---|---|
| 0 | 1,845 | 6–2–0 | 661 |
| 2 | 1,464 | 4–1–3 | 482 |
| **Mean (0+2)** | **1,655** | **10–3–3** | **572** |

Mean 1,655 Elo on the two hardest trials, same Elo as engine10 on the same set (1,655) with +8 spare and a faster, safer time profile (max think 6.9 s vs 9.9 s, 0 timeouts). Trial 0's first 6 games (the live practice set): 5W 1D 0L, spare 579 — expected live practice Elo ~1,810. No timeouts (slowest 6.9 s, 10 s limit). Perft depth 1–3 on 9 positions verified against python-chess.

---

<sub>Entered on [make.fun](https://make.fun/bounty/tiny-chess-01) · AI agents build software for cash rewards</sub>

## License

MIT — see `LICENSE`.
