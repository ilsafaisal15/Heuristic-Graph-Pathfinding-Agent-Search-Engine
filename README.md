# Heuristic Graph Pathfinding Agent Search Engine

A search framework in which a virtual agent finds paths through grid mazes. **A\*** (two heuristics) and **Dijkstra** are implemented from scratch and compared across obstacle densities.

## Requirements

- Google Colab or Python 3 with `numpy`, `pandas`, `matplotlib` (all preinstalled in Colab). No internet or GPU needed.

## How to run

1. Upload `pathfinding_agent.ipynb` to [colab.research.google.com](https://colab.research.google.com).
2. Choose Runtime > Run all.
3. At the end `pathfinding_results.zip` downloads automatically. Outside Colab the files stay in `./outputs`.

## How it works

- **Grid:** 0 = free cell, 1 = obstacle. 4-connected moves, cost 1 per move. Start is the top-left corner, goal the bottom-right.
- **Search:** one best-first search on a binary heap (`heapq`) with f(n) = g(n) + h(n), a closed set and parent pointers.

| Algorithm | Heuristic h(n) |
|---|---|
| Dijkstra | 0 |
| A\* (Manhattan) | abs(dx) + abs(dy) |
| A\* (Euclidean) | sqrt(dx² + dy²) |

## Settings you can change

In the experiment cell: `SIZE = 50`, `DENSITIES = [0.10, 0.20, 0.30, 0.35, 0.40]`, `TRIALS = 20` (random grids per density, seeds 0 to 19).

## Outputs (in `outputs/`)

| File | Content |
|---|---|
| `experiment_log.csv` | One row per run: steps, expanded nodes, expanded-node density, runtime, solved or not |
| `summary_metrics.csv` | Averages per density and algorithm (solved grids only) |
| `path_visualization_density_0.2.png`, `..._0.3.png` | Maze with explored cells (light blue) and final path (red) for each algorithm |
| `metrics_comparison.png` | Expanded nodes, runtime and path steps versus density |

## Key results (50 × 50 grids, 20 per density)

- All three algorithms return identical, optimal path lengths on every solvable grid.
- A\* (Manhattan) expands 12%, 37%, 59% and 71% fewer nodes than Dijkstra at densities 0.10, 0.20, 0.30 and 0.35, with runtime speed-ups of 1.08×, 1.57×, 2.47× and 3.38×.
- A\* (Euclidean) is weaker: 2% to 41% fewer nodes.
- Solvable grids: 20, 20, 10, 2 and 0 of 20 at densities 0.10, 0.20, 0.30, 0.35 and 0.40.

## Limitations

- Averages at densities 0.30 and 0.35 rest on only 10 and 2 solvable grids.
- Random obstacles, not structured mazes; 4-connected, uniform-cost moves.
- Runtimes are single-machine Python timings; compare ratios rather than absolute milliseconds.
- Q-Learning was not implemented.
