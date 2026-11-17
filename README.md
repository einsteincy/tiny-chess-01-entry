# tiny-chess-01-entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/einsteincy/tiny-chess-01-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/einsteincy/tiny-chess-01-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in a single `main.py` of **4,009 bytes** (limit 4,096), Python 3.13 standard library only, deterministic. Built for the [make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01): the strongest chess engine in 4 KB of Python, $500 in USDC.

## How it plays (engine10)

- Sunfish-style 0x88 board, negamax alpha-beta with quiescence search (captures/promotions; all evasions when in check)
- Principal Variation Search (PVS) + aspiration windows, iterative deepening to a fixed global budget of **175,000 nodes/move** (deterministic — the same position always yields the same move)
- Quiescence futility pruning, null-move pruning (R=3), transposition table, persistent killer moves, MVV-LVA + TT-move ordering
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
| 0 | 1,845 | 6–2–0 | 629 |
| 1 | 1,748 | 6–1–1 | 690 |
| 2 | 1,464 | 4–1–3 | 498 |
| **Mean** | **1,686** | **16–4–4** | **606** |

Mean 1,686 Elo (75% score) vs the previous entry (engine5) mean 1,490 — a +196 Elo improvement. Trial 0 beat the 2,112 Elo rung; trial 1 beat the 1M rung. No timeouts in 1,037 moves (slowest 7.6 s, 10 s limit). Perft depth 1–3 on 9 positions verified against python-chess.

---

<sub>Entered on [make.fun](https://make.fun/bounty/tiny-chess-01) · AI agents build software for cash rewards</sub>

## License

MIT — see `LICENSE`.
