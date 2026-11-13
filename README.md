# tiny-chess-01 entry

A 4 KB Python chess engine for the make.fun tiny-chess-01 bounty:
the strongest engine in 4 KB of Python, graded over 8 games against
fixed house engines (depth-2/3/4 and nodes-100k to nodes-2M).

## The engine

`main.py` (3,084 bytes) is the whole engine: a packed LZMA payload of
a readable source, unpacked and run at start. It plays through the env
(`MAKEFUN_ENV_URL`), one move per call, deterministic throughout.

- 0x88 board, pseudo-legal generation with a legality filter.
- Negamax with alpha-beta, PVS, iterative deepening to a fixed
  150,000-node budget per move (never a clock), quiescence over
  captures and promotions, all evasions when in check.
- Move ordering: transposition table, MVV-LVA captures, two killer
  moves per ply, history heuristic.
- Evaluation: material plus Michniewski piece-square tables,
  endgame king centralisation interpolated by non-pawn material,
  bishop-pair bonus. Repetition inside the search scores 0.
- Deterministic: fixed node budget, no randomness, no clock.

## Practice results

Three practice trials (kit env server + scorer), mean Elo 1290
against the 1154 practice floor:

- trial-0: Elo 1530 (5 won, 0 drawn, 3 lost)
- trial-1: Elo 1087 (2 won, 0 drawn, 6 lost)
- trial-2: Elo 1253 (3 won, 0 drawn, 5 lost)

Perft passes on the standard positions; mates, promotions,
castling and en passant verified locally.

## Size rule

- `main.py` is at most 4,096 bytes; the whole repository is at
  most 8,192 bytes; no file over 4,096. MIT licensed.
