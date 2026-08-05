# DCSA Influence Maximization Research

This folder contains the implementation and result files for a Social Network Analysis research project on influence maximization. The main notebook compares the original DCSA approach with an improved DCSA version and other baseline algorithms.

## Project Files

- `Manish Code_fitness_stagnation_localSearch_Improved.ipynb` - main notebook containing the original DCSA, improved DCSA, CELF, DPSO, Independent Cascade evaluation, graph loading, benchmarking, and plotting code.
- `Original Research Paper.pdf` - base/reference paper.
- `Research paper SNA Manish Neeraj.pdf` and `.docx` - written research paper/report material.
- `DCSA - improvements Presentation.pdf` - presentation explaining the improvements.
- `result/` - generated charts and CSV result files.

## Problem Statement

The project studies influence maximization in social networks. Given a graph and a seed budget `k`, the goal is to select `k` influential nodes that maximize the expected spread of information under the Independent Cascade model.

## Algorithms Used

### Original DCSA

The original DCSA implementation follows the crow search based optimization idea. It maintains a population of candidate seed sets, updates memory, performs random walks, and uses Local Influence Estimation to score candidate solutions.

### Improved DCSA

The improved DCSA version adds:

- cached Local Influence Estimation to reduce repeated computation,
- improved probabilistic two-hop influence estimation,
- hybrid population initialization,
- lightweight local search,
- adaptive awareness probability during stagnation,
- partial restart of weak candidates when stagnation continues.

The main purpose of these changes is to reduce runtime while maintaining or improving influence spread.

### CELF

CELF is used as a greedy baseline for influence maximization. It estimates marginal gain using the Independent Cascade model and uses lazy evaluation to reduce unnecessary recomputation.

### DPSO

DPSO is used as another metaheuristic baseline. It applies a discrete particle swarm style search to optimize the seed set.

## Evaluation Metrics

- `LIE_score` - estimated influence score from Local Influence Estimation.
- `IC_spread` - average influence spread measured using the Independent Cascade model.
- `time_sec` - runtime in seconds.
- `seeds` - selected seed nodes.

## Result Images

### Independent Cascade Spread - CaAstroPh

![Independent Cascade Spread CaAstroPh](result/ic_spread(CaAstroPh).png)

### Independent Cascade Spread - CondMat

![Independent Cascade Spread CondMat](result/ic_spread(CondMat).png)

### Independent Cascade Spread - HepTh

![Independent Cascade Spread HepTh](result/ic_spread(Hepth).png)

### Original DCSA Result Plots

![Original DCSA Result 1](result/original/download%20(2).png)

![Original DCSA Result 2](result/original/download%20(3).png)

### Improved DCSA Result Plots

![Improved DCSA Result 1](result/optimised/download%20(2).png)

![Improved DCSA Result 2](result/optimised/download%20(3).png)

## Saved CSV Results

### Improved DCSA

| k | LIE_score | IC_spread | time_sec |
|---:|---:|---:|---:|
| 5 | 8.4399 | 8.39 | 1.03 |
| 10 | 15.587 | 16.2533 | 1.34 |
| 15 | 22.3702 | 23.0933 | 1.55 |
| 20 | 29.0737 | 30.03 | 1.85 |
| 25 | 35.3711 | 36.7333 | 3.12 |
| 30 | 42.1516 | 44.41 | 2.24 |
| 35 | 48.0566 | 49.4633 | 3.58 |
| 40 | 54.8951 | 56.89 | 5.11 |
| 45 | 61.3367 | 63.46 | 6.58 |
| 50 | 68.1606 | 70.7467 | 5.74 |

### Original DCSA

| k | LIE_score | IC_spread | time_sec |
|---:|---:|---:|---:|
| 5 | 8.4554 | 8.3633 | 99.55 |
| 10 | 15.2415 | 15.4567 | 265.97 |
| 15 | 21.4686 | 21.94 | 366.24 |
| 20 | 28.4784 | 29.22 | 602.49 |
| 25 | 34.419 | 35.6967 | 554.44 |
| 30 | 40.4551 | 41.5933 | 721.82 |
| 35 | 46.9803 | 48.35 | 762.62 |
| 40 | 53.1168 | 54.56 | 963.61 |
| 45 | 60.2167 | 62.1533 | 1277.3 |
| 50 | 66.0554 | 68.72 | 1364.48 |

## Result Summary

Based on the saved CSV files, the improved DCSA is much faster than the original DCSA. For example, at `k=50`, the saved improved runtime is `5.74` seconds, while the saved original runtime is `1364.48` seconds. The saved Independent Cascade spread is also slightly higher for the improved version in most rows.

This supports the main project claim that the improved DCSA can reduce computation time while keeping influence spread competitive.

## Important Validation Note

The current saved CSV result files need to be regenerated before final submission. For rows where `k > 10`, the `seeds` column contains only 10 seed nodes even though the requested seed budget is larger. This means the spread/runtime trend is useful for discussion, but the saved CSV files should not be treated as fully verified final experimental output until the benchmark is rerun and each row satisfies:

```text
number of selected seeds == k
```

The notebook also uses Google Colab dataset paths such as `/content/CA-HepTh.txt`. To run locally, place the dataset text files in this folder or update the paths in the notebook.

## How to Reproduce

1. Open `Manish Code_fitness_stagnation_localSearch_Improved.ipynb`.
2. Install required Python packages if needed:

```bash
pip install networkx numpy pandas matplotlib
```

3. Update the dataset paths in the `dataset_files` dictionary.
4. Run all notebook cells from top to bottom.
5. Regenerate the CSV files and result plots.

## Recommended Fixes Before Final Use

- Regenerate all results and confirm every seed set has exactly `k` unique nodes.
- Fix the LIE cache key so it also includes the propagation probability `p`.
- Avoid redefining `IC_model` twice in the notebook.
- Replace Colab-only dataset paths with local relative paths.
