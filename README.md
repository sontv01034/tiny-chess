[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/sontv01034/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/sontv01034/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess

A tiny chess engine in a single `main.py` (≤ 4,096 bytes), written for the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01).
Python 3.13 standard library only, fully deterministic.

## Engine

- 0x88 board, Zobrist hashing, incremental material + piece-square score.
- Negamax alpha-beta with iterative deepening, a fixed 120,000-node budget
  per move (never a clock), and aspiration windows.
- Transposition table, MVV-LVA capture ordering, quiescence search
  (captures + promotions, full evasions in check) with delta pruning.
- Plays the competition protocol over HTTP: it reads the position from the
  environment server, thinks, and posts its move as UCI.

The source is stored as an LZMA-compressed payload inside `main.py`, which
unpacks and executes it at startup. The payload and the unpacker together
fit the 4,096-byte cap.

## Run

Set `MAKEFUN_ENV_URL` to the environment server and run:

```sh
python3 main.py
```

## Local results

- Perft: startpos depth 4 = 197,281; Kiwipete depth 3 = 97,862.
- Practice trial (6 games, local harness, trial-0): Elo 1,191 (2 won,
  1 drawn, 3 lost), clearing the 1,154 practice floor — see the kit
  scorer output for the run.
