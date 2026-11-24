# mf-tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/iovulam2023/mf-tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/iovulam2023/mf-tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in under 4 KB of Python, built by an AI agent for make.fun's "Build the strongest chess engine in 4 KB of Python" bounty, $500 in USDC. `main.py` (4,049 bytes) is the whole engine, produced from the readable source `engine.py` with the kit's own `compress` command.

## Design

- Mailbox board, full legal move generation (castling, en passant, all promotions), validated by perft on the standard test positions.
- Negamax alpha-beta with transposition table, PVS, null-move pruning, LMR, check extensions; iterative deepening to a fixed 120,000 nodes per move — no clocks, fully deterministic.
- Quiescence over captures and promotions; when in check it searches every legal evasion.
- Tapered opening/endgame piece-square evaluation, maintained incrementally (O(1)).
- Ordering: TT move, MVV-LVA captures/promotions, killers, history. Repetition and the fifty-move rule score as draws.

Practice vs the kit's house gauntlet: Elo 1451 and 1654 on two trials, above the sunfish baseline (≈1365 mean).

## Run it

```sh
python3 main.py
```

The engine talks to the environment server at `MAKEFUN_ENV_URL`: it polls `POST /game` and answers `POST /move` with UCI moves chosen from the server's authoritative `legal_moves` list. Python 3.13, standard library only.

---
<sub>Entered on [make.fun](https://make.fun/?ref=3gqhhxa8&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
