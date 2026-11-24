# tiny-chess — a chess engine in under 4 KB of Python

An original engine written for the make.fun tiny-chess-01 bounty.
`main.py` (4,049 bytes) is the whole engine; it is produced from the
readable source `engine.py` with the kit's own `compress` command.

## Design

- Mailbox board (64 squares), full legal move generation: castling,
  en passant, all promotions — validated by perft on the standard
  test positions.
- Negamax alpha-beta search with a transposition table, principal
  variation search, null-move pruning, late move reductions, check
  extensions and iterative deepening to a fixed budget of 120,000
  nodes per move. No clocks anywhere: two runs of the same game
  produce identical moves.
- Quiescence search over captures and promotions; when in check it
  searches every legal evasion instead of standing pat.
- Tapered (opening/endgame) piece-square evaluation, maintained
  incrementally with make/unmake, so evaluation is O(1).
- Move ordering: transposition-table move first, then captures by
  MVV-LVA and promotions, then killer moves and history scores.
- Repetition and the fifty-move rule score as draws, using the
  game's position history plus the halfmove clock.

## Interface

The engine talks to the environment server at `MAKEFUN_ENV_URL`:
it polls `POST /game` and answers `POST /move` with UCI moves,
always choosing from the server's authoritative `legal_moves` list.
