# 0005 A picture of the shuffled painting

**Status:** implemented
**Date:** 2026-11-03

## Goal

The frame starry puts on its readme is held against a picture of what that
frame should be, so a change to the tiles, the tray or the painting on them
is seen rather than shipped.

## Why it belongs here

Spec 0001 says what the board is and the specs after it say how it slides,
what a hint is and how a tile lifts. Every test of them is arithmetic: which
cell holds which tile, whether a board is solved, how many moves a hint is
worth. None of them can see the painting, and the painting is most of what
this game is.

The engine's spec 0076 made the rest one call.

**The staging is a seed and nothing else.** The opening frame is already the
picture: the painting cut up and shuffled in its tray, which is where every
run begins. A staged run stages nothing and only stops what would move.

## Behavior

**`StarryGame::new` takes `staged`, and a staged run scrambles from
`POSED_SEED`.** The board was scrambled from entropy, so no two runs agreed:
every regenerated screenshot was a different board and a picture test could
never have held against one. A seeded `StdRng` on the staged path alone
fixes that and leaves a real run as random as it was.

This changes what `refresh-screenshots` photographs, so starry's committed
screenshot moves the next time it is taken.

**One `update`, at no time at all**, which clears the motion and lays the
tray out.

**It is the engine's size**, 320 by 240, said once in `picture::SIZE`.

## Acceptance criteria

- The shuffled painting matches its
  picture. — `starry_game::tests::the_shuffled_painting_looks_right`

### Verified by hand

- **The picture itself.** Fifteen tiles of the painting in the tray with
  one cell empty, the cypress and the village and the moon each cut across
  tile edges, which is what makes the puzzle readable as a painting.
- **A control.** The seed moved from 7 to 8 and the test failed on 50.8% of
  its pixels, which is what a different scramble of the same painting looks
  like.

## Out of scope

**Which seed.** Any of them deals a scramble. This one is the first tried.

**A picture of a solved board.** That is spec 0001's arithmetic, and a
solved board is the painting, which the repo already has a copy of.
