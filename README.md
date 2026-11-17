[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/jijiaichimi/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/jijiaichimi/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-01 entry

`main.py` is a complete chess engine in 4,076 bytes: negamax alpha-beta
search with iterative deepening, principal-variation search, a
transposition table with bound flags, check extensions, and a quiescence
search over captures and check evasions. Moves are generated on a 0x88
board; evaluation is incremental (material plus pawn, knight and bishop
piece-square tables) with Zobrist keys, repetition and fifty-move draws
scored as zero. The search stops at a fixed budget of 45,000 nodes per
move — never a clock — so trials replay move for move. The readable
source is kept beside the packed entrypoint in the submission bundle;
`main.py` itself is the `makefun compress` output of that source.

Local practice on the kit gauntlets (6 games per trial, house anchors
855–1596): the full-size prototype reached Elo 1545. In its final full
local run the packed build scored 1 win, 3 draws and 2 losses on the
trial-2 gauntlet, every move comfortably inside the 10-second limit; a
bishop piece-square table was restored afterwards, inside the same
byte budget, and the result smoke-tested clean.

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed,
  Python 3.13, standard library only. It reads, imports or execs no other
  file in the repository.
- Every byte counts: comments, whitespace, docstrings.
- Compression is allowed and counted. A `main.py` that unpacks and execs
  a compressed payload (`lzma`, `zlib`) is fine; the payload and the
  unpacker are both part of its 4,096 bytes.
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes in
  all, and no file over 4,096. MIT, BSD, ISC and 0BSD licences fit.
- Search to a fixed depth or node count, never a clock: a final
  contender's trials are replayed and must play the same moves.

## Grade it locally

You need Docker and the `makefun` CLI. Download the competition's dev
kit, unpack it, and from inside it run:

    makefun grade-local --kit . --bot <your repository> --driver docker

It plays one practice trial against the house engines, replays it, and
prints the Elo the scorer gives it. Before you submit, `makefun check`
runs the site's own pre-submit checks, size caps included.
