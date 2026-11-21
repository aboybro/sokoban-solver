# Sokoban Solver

A push-optimal Sokoban solver, written for the make.fun bounty
[Build the best Sokoban solver, 21 November 2026](https://make.fun/bounty/sokoban-solver-01-run-20261121)
(`sokoban-solver-01-run-20261121`).

`main.ts` answers all 5 instances of a set in one run (Node 22, no
dependencies, no network, no clock): it reads `/input/<id>/level.txt` and
writes `/output/<id>/solution.txt` with the player's moves in LURD.

## How it works

A level from this competition is a one-square-wide hallway with one to four
rooms off it, each room holding 3 or 4 boxes and as many goals, opening onto
the hallway through a single door. A box pushed out of a door into the
hallway can never be pushed back, so every box stays in its room and the
level's push-optimal count is the sum of its rooms' optima. The solver
therefore splits a level into its rooms and solves each room on its own,
push-optimally, then joins the rooms' move strings.

Each room is solved by a breadth-first search over pushes. A state is the
boxes' squares together with the region the player can walk in (named by its
lowest square). States expand in order of pushes made, so the first solved
state met is push-optimal for that room. Two deadlocks are pruned, neither of
which can cut a solvable state: dead squares (found by pulling boxes back
from the goals over open floor) and frozen 2 x 2 blocks of walls and boxes.
The search is bounded by a count of states kept, never by the clock, so two
runs write the same bytes; the whole run is bounded by work as well, so a
pathological set can cost unsolved instances, never the whole run.

## Results

- Practice set: **1,000,000** — all 5 instances push-optimal (13, 16, 29, 47
  and 60 pushes), verified with the kit's `grade-local` and by replaying the
  solutions against the practice manifest.
- Six further sets drawn from the kit's generator (other seeds): **1,000,000**
  on each, all instances push-optimal, about 1–2 seconds a set.
- Determinism: two runs on the practice set write byte-identical outputs.

## License

MIT — see [LICENSE](LICENSE).

<!-- SPDX-License-Identifier: MIT -->
