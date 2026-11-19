# Heuristic Graph Pathfinding Agent Search Engine

A search framework that lets a virtual agent navigate grid mazes. **A\*** (Manhattan and Euclidean heuristics) and **Dijkstra** are implemented from scratch on a binary heap, then benchmarked across obstacle densities on step count, runtime and expanded-node density.

![Search behaviour of Dijkstra, A* Manhattan and A* Euclidean on one grid](assets/path_visualization_density_0.2.png)

*One 50 × 50 grid at obstacle density 0.20. Light blue = expanded cells, red = final path. All three find a 98-step path; Dijkstra expands 1,961 nodes, A\* (Manhattan) 1,027, A\* (Euclidean) 1,708.*

## Features

- Dijkstra and A\* from scratch (`heapq`, closed set, parent pointers), one shared best-first search where only the heuristic changes
- Random grid generator with a seed and obstacle density
- Metrics per run: path steps, expanded nodes, expanded-node density, runtime (ms)
- Experiment sweep over densities with CSV logs and plots
- Visualizations of explored cells and final paths

## Algorithms

| Algorithm | Heuristic h(n) | Notes |
|---|---|---|
| Dijkstra | 0 | Uninformed, expands outward in all directions |
| A\* (Manhattan) | abs(dx) + abs(dy) | Admissible and consistent on a 4-connected grid |
| A\* (Euclidean) | sqrt(dx² + dy²) | Admissible, but a weaker estimate than Manhattan here |

Grid: `0` = free, `1` = obstacle. Moves are 4-connected with cost 1. Start is the top-left corner, goal the bottom-right.

## Quick start

**Colab:** click the badge above (replace `YOUR_USERNAME/YOUR_REPO` in the link with your repository path), then choose Runtime > Run all.

**Local:**

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO/task2-pathfinding
pip install -r requirements.txt jupyter
jupyter notebook pathfinding_agent.ipynb
```

Run all cells. Results are written to `outputs/` (and zipped to `pathfinding_results.zip`; the automatic download step only works in Colab).

## Configuration

Edit these values in the experiment cell:

```python
SIZE = 50                                   # grid is SIZE x SIZE
DENSITIES = [0.10, 0.20, 0.30, 0.35, 0.40]  # obstacle densities
TRIALS = 20                                 # random grids per density (seeds 0..19)
```

## Outputs

| File | Content |
|---|---|
| `outputs/experiment_log.csv` | One row per run (steps, expanded nodes, node density, runtime, solved) |
| `outputs/summary_metrics.csv` | Averages per density and algorithm (solved grids only) |
| `outputs/path_visualization_density_*.png` | Explored cells and final paths |
| `outputs/metrics_comparison.png` | Expanded nodes, runtime and steps versus density |

## Results

50 × 50 grids, 20 random grids per density. All three algorithms return **identical, optimal path lengths** on every solvable grid.

| Density | Solvable grids | A\* (Manhattan): fewer nodes than Dijkstra | A\* (Euclidean): fewer nodes | A\* (Manhattan) runtime speed-up |
|---|---|---|---|---|
| 0.10 | 20 / 20 | 12.0% | 2.1% | 1.08× |
| 0.20 | 20 / 20 | 37.0% | 9.5% | 1.57× |
| 0.30 | 10 / 20 | 58.6% | 26.0% | 2.47× |
| 0.35 | 2 / 20 | 71.5% | 41.0% | 3.38× |
| 0.40 | 0 / 20 | n/a | n/a | n/a |

![Average expanded nodes, runtime and path steps by obstacle density](assets/metrics_comparison.png)

**Takeaways**

- The Manhattan heuristic keeps Dijkstra's optimality while expanding far fewer nodes, and the saving grows with obstacle density.
- Euclidean distance never exceeds Manhattan distance, so it guides the search less strongly and expands more nodes.
- Random grids stop being solvable near density 0.40, consistent with the square-lattice percolation threshold (open cells need roughly 0.59 density).

## Limitations

- Averages at densities 0.30 and 0.35 use only 10 and 2 solvable grids, so they are indicative, and they may favour A\* because only solvable grids are counted.
- Random obstacles, 4-connected uniform-cost moves, no structured mazes.
- Runtimes are single-machine Python timings; compare ratios rather than absolute milliseconds.
- Q-Learning is not implemented.

## Repository layout

```
task2-pathfinding/
├── pathfinding_agent.ipynb   # notebook: algorithms, experiments, plots, export
├── requirements.txt
├── assets/                   # images used in this README
└── outputs/                  # created when the notebook runs
```
