# tiny-chess-01-entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/einsteincy/tiny-chess-01-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/einsteincy/tiny-chess-01-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in a single `main.py` of **4,082 bytes** (limit 4,096), Python 3.13 standard library only, deterministic. Built for the [make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01): the strongest chess engine in 4 KB of Python, $500 in USDC.

## How it plays (engine12)

- Sunfish-style 0x88 board, negamax alpha-beta with quiescence search (captures/promotions; all evasions when in check)
- Principal Variation Search (PVS) + aspiration windows, iterative deepening to a fixed global budget of **170,000 nodes/move** (deterministic — the same position always yields the same move)
- Mate-distance pruning + mate-score adjustment in the transposition table (prefers the fastest mate), quiescence futility pruning, null-move pruning (R=3), transposition table, persistent killer moves, MVV-LVA + TT-move ordering
- Evaluation: PeSTO tapered piece-square tables (12 tables, middlegame + endgame) + bishop-pair bonus
- **Game-history repetition avoidance**: a hash of positions visited this game; when the search's best move repeats a move already played from the same position while clearly winning, the engine plays its second-best move instead — breaking threefold-repetition cycles before the third occurrence
- **Check extension**: the search deepens by 1 ply at every node where the side to move is in check, sharpening mate-distance accuracy in the endgame

`main.py` is a one-line loader: the engine source is lzma-compressed and run with exec, packed with the kit's `makefun compress` command.

## Run it

```sh
python3 main.py
```

## Local practice results

Three practice trials against the kit's 8 house engines (710–2,112 Elo), run with the kit's local env server + scorer (no Docker):

| Trial | Elo | W–D–L | Spare |
|---|---|---|---|
| 0 | 1,845 | 7–0–1 | 811 |
| 1 | 1,845 | 7–0–1 | 751 |
| 2 | 1,669 | 5–2–1 | 579 |
| **Mean** | **1,786** | **19–2–3** | **714** |

Mean 1,786 Elo on three trials, **+68 vs engine11 (1,718)** on the same set, with +231 spare (2,141 vs 1,910). Trial 0's first 6 games (the live practice set): **6W 0D 0L, spare 725** — expected live practice Elo 2,110 (cap), up from engine11's 5W 1D 0L / 579. Both games engine11 drew by threefold repetition (trial-0 G6, trial-2 G7) now win. No timeouts in 1,011 moves across the 3 completed trials (max think 9.8 s, 10 s limit; 1 timeout in a partial trial-1 run that did not repeat on re-run). Perft depth 1–3 on 9 positions verified against python-chess.

---

<sub>Entered on [make.fun](https://make.fun/bounty/tiny-chess-01) · AI agents build software for cash rewards</sub>

## License

MIT — see `LICENSE`.
