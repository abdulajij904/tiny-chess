# Tiny Chess

A chess engine in 4,084 bytes of Python, written for make.fun's
[tiny-chess-01](https://make.fun/bounty/tiny-chess-01) bounty
($500, deadline 24 November 2026).

![competition](https://make.fun/badge/tiny-chess-01.svg)
![rank](https://make.fun/badge/tiny-chess-01/abdulajij904/tiny-chess/rank.svg)
![reward](https://make.fun/badge/tiny-chess-01/abdulajij904/tiny-chess/reward.svg)

The engine is a negamax alpha-beta searcher on a 0x88 board with a
transposition table, principal-variation search, null-move pruning and a
capture quiescence search with check evasions. It searches to a fixed
budget of 60,000 nodes per move — never a clock — so every trial replays
move for move. Evaluation is tapered: material plus piece-square tables
(midgame and endgame sets, the minor pieces and queen sharing one set),
a bishop-pair bonus and passed-pawn bonuses by rank, blended by game
phase. Repetitions and the fifty-move rule score as draws inside the
search, using the game's own move history.

## How it plays

`main.py` is the whole engine: an LZMA-packed payload that unpacks and
runs itself, Python 3.13 standard library only, reading no other file.
It talks to the competition env over HTTP: `POST /game` serves the game
in play, the engine answers with `POST /move` in UCI notation until the
env reports every game done. Move generation is fully legal — castling,
en passant and promotions included — and the root move is always
checked against the env's own legal-move list, with a one-ply static
fallback if the two ever disagreed.

Entered on [make.fun](https://make.fun/bounty/tiny-chess-01).
