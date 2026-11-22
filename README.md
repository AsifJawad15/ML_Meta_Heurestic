# ML Meta-Heuristic Optimization: GA and ABC on the Ackley Function

Two meta-heuristic optimizers, a **Genetic Algorithm (GA)** and **Artificial Bee Colony Optimization (ABCO)**, each minimizing the 2-D **Ackley benchmark function** in Jupyter notebooks (Machine Learning course assignment).

## Problem

Minimize the 2-D Ackley function on `-5 <= x1, x2 <= 5`:

    f(x) = -20 exp(-0.2 sqrt(mean(x_i^2))) - exp(mean(cos(2 pi x_i))) + 20 + e

The global minimum is `f(0, 0) = 0`. Ackley is **multimodal** (many local minima), so it tests whether an optimizer can escape local basins and still refine its answer.

## Notebooks

| Notebook | Algorithm | Key settings |
|---|---|---|
| [`GA_Ackley_Genetic_Algorithm.ipynb`](GA_Ackley_Genetic_Algorithm.ipynb) | Genetic Algorithm | population 50, 100 generations, real-valued chromosomes `[x1, x2]`, roulette-wheel selection, one-point crossover (p = 0.80), Gaussian mutation (p = 0.05, size decays over time), 1 elite |
| [`ABCO_Ackley_Artificial_Bee_Colony.ipynb`](ABCO_Ackley_Artificial_Bee_Colony.ipynb) | Artificial Bee Colony | 50 food sources, 50 employed + 50 onlooker bees, 100 cycles, abandonment limit 20, greedy replacement, scout bees |

Both notebooks contain the objective function, a modular implementation, convergence plots, a search-space visualization, a 10-seed reliability experiment, and automatic correctness checks (`assert`).

## How each algorithm works

**GA** evolves a population: *selection* (better chromosomes are more likely to be parents), *crossover* (children mix `x1` and `x2` from two parents), *mutation* (random Gaussian changes whose size shrinks from exploration to fine-tuning), and *elitism* (the best chromosome always survives).

**ABCO** imitates honey-bee foraging. *Employed bees* search a neighbor of their food source with `v_ij = x_ij + phi (x_ij - x_kj)`. *Onlooker bees* choose sources in proportion to quality `1 / (1 + f)`. *Scout bees* replace a source that has failed too many times. The best-ever solution is stored separately. The ABCO notebook also builds an animated simulation of the colony searching the landscape.

## Results (seeded runs, outputs saved in the notebooks)

| | GA | ABCO |
|---|---|---|
| Single run (seed 42) best `f` | 3.32e-04 | 7.56e-11 |
| Distance to `(0, 0)` | 1.17e-04 | 2.67e-11 |
| Stop reason | 100 generations | target `1e-10` reached at cycle 79 |
| 10 seeds, mean best `f` | 3.17e-04 | 6.59e-11 |
| 10 seeds, std dev | 1.76e-04 | 1.95e-11 |

Both find the global minimum region reliably. With these settings, ABCO reaches a much lower objective value on this 2-D problem. This is a comparison on one benchmark with one parameter set, not a general claim.

## Run it

```bash
pip install numpy matplotlib pillow jupyter
jupyter notebook
```

Open either notebook and choose *Run All*. Seeds are fixed, so results are reproducible.

## References

- Karaboga, D. and Basturk, B. (2007). A powerful and efficient algorithm for numerical function optimization: artificial bee colony (ABC) algorithm. *Journal of Global Optimization*, 39, 459-471. https://doi.org/10.1007/s10898-007-9149-x
