# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/cytoboth/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/cytoboth/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in a single 4,096-byte `main.py`: negamax with alpha-beta,
iterative deepening on a fixed 130,000-node budget, transposition table,
killer + history move ordering, MVV-LVA captures, quiescence search and a
material + piece-square evaluation. Written from scratch in Python 3.13
standard library only; no model calls, no clock — fully deterministic.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=fd3yvszx&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>

## The size rule

- `main.py` is the whole engine: 3,299 bytes as committed. It is the
  kit's `makefun compress` output (an LZMA stream of the readable source
  with a small stub); the payload and the stub are both part of its
  4,096 bytes. Compression is allowed by the competition rules.
- The rest of the tree (this README, LICENSE, `.makefun/owner.json`) is
  at most 8,192 bytes in all, and no file is over 4,096. License: MIT.

## How it plays

- 0x88 board, pseudo-legal move generation with make/unmake and a
  king-safety test; handles castling, en passant, promotion, checkmate
  and stalemate. Move generation verified by perft on six standard
  test positions (all depths to 4, exact counts).
- Search: negamax, fail-soft alpha-beta, iterative deepening to depth 40
  on a fixed node budget (130,000 nodes/move), transposition table with
  exact/lower/upper flags, killer moves, history heuristic, MVV-LVA
  capture ordering, check extensions, quiescence over captures,
  promotions and check evasions.
- Evaluation: material (P100 N320 B330 R500 Q900) + Michniewski
  piece-square tables, bishop-pair bonus, middlegame/endgame king tables,
  insufficient-material draw detection.

## Grade it locally

From the kit directory (no Docker needed):

    node cli/makefun.mjs grade-local --kit . --bot <this repository> --driver process

It plays one practice trial against the house engines and prints the Elo
the scorer gives it.
