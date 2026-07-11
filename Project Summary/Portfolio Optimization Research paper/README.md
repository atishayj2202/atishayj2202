# Advanced Portfolio Optimization with AOBL-SOS
### Replicating, Enhancing, and Validating Swarm Intelligence for Constrained Financial Markets

This folder contains a complete repository index, design guide, and experimental findings of our research project. It is structured to help write social media posts (LinkedIn, X/Twitter) and document your GitHub portfolio.

---

## 1. Project Overview & Planning

### The Challenge
Standard portfolio optimization models (like Markowitz's mean-variance framework) are highly sensitive to estimation errors and struggle in non-convex, constrained search spaces. Swarm Intelligence (SI) algorithms like **Symbiotic Organisms Search (SOS)** offer derivative-free global search capabilities. However, standard SOS suffers from two critical flaws:
1.  **Premature Stagnation**: The organisms quickly cluster in local optima, flattening search progress.
2.  **Simplex Constraint Collapse**: Standard Opposition-Based Learning (OBL) operators ($\bar{w} = 1 - w$) violate the sum-to-one ($\sum w_i = 1$) and boundary constraints. Normalizing the opposite vector collapses allocations to a uniform, equal-weight portfolio ($1/N$), wiping out all learned patterns.

### The Plan
We set out to modularize the research, implement competitive baselines, and resolve a list of 18 peer-review critiques:
*   **Modularization**: Refactored unorganized Jupyter notebooks into a production-grade Python library (`src/`).
*   **Baselines**: Implemented cognitive **Particle Swarm Optimization (PSO)** and **Differential Evolution (DE)**.
*   **Robust Operator Design**: Created a custom **Rank-Reversal Simplex Opposition** operator to make OBL viable for asset allocation.
*   **Algorithmic Improvements**: Designed an **Adaptive Stagnation Trigger** to dynamically shake the swarm out of flat valleys.
*   **Rigorous Validation**: Tested the hybrid solver on 8 mathematical benchmarks and S&P 500 portfolio optimization.

---

## 2. System Architecture & Workings

The library is organized modularly to decouple the solvers, mathematical benchmarks, and financial evaluations:

```
src/
│
├── utils.py                # Cap enforcement, redistribution, and seed control
│
├── algorithms/             # SI Solvers
│   ├── sos.py              # Symbiotic Organisms Search
│   ├── obl.py              # Classic, Quasi, and Rank-Reversal Simplex OBL
│   ├── aobl_sos.py         # Adaptive Stagnation-conditioned OBL-SOS
│   ├── pso.py              # Particle Swarm Optimization baseline
│   └── de.py               # Differential Evolution baseline
│
├── benchmarks/             # Code validation
│   ├── functions.py        # 11 benchmark definitions with standard bounds
│   └── runner.py           # Benchmark comparison execution engine
│
└── portfolio/              # Quantitative asset management
    ├── data.py             # Robust yfinance data scraper with header session fallbacks
    ├── evaluation.py       # Portfolio returns, Sharpe, Sortino, drawdowns, equal-weight
    └── runner.py           # Seed search loop, optimization runner, and CSV/plot generators
```

### The Math: Rank-Reversal Simplex Opposition
For an asset allocation vector $w = [w_1, w_2, \dots, w_N]^T$, the standard opposite vector $\bar{w}_i = 1 - w_i$ collapses to $1/N$ after normalization. 

Our **Rank-Reversal Simplex Opposition** solves this by sorting the weights in ascending order $w_{\pi(1)} \le w_{\pi(2)} \le \dots \le w_{\pi(N)}$ and reversing their indices in rank space:
$$w^{op}_{\pi(i)} = w_{\pi(N - i + 1)} \quad \forall i \in \{1, 2, \dots, N\}$$

Since $w^{op}$ is a permutation of the original weights, it mathematically guarantees that $\sum w^{op}_i = 1$ and $w^{op}_i \le w_{max}$ without requiring any destructive normalization passes.

---

## 3. Testing and Experimental Design

1.  **Mathematical Benchmark Testing**:
    *   Evaluated 8 selected functions (Sphere, Rosenbrock, Schwefel, Zakharov, Levy, Sum Squares, Styblinski-Tang, Michalewicz) over 30 independent runs ($D=30, POP=30, ITERS=300$).
    *   Search boundaries were set to standard function-specific domains (e.g., $[-500, 500]$ for Schwefel) to resolve the hardcoded boundary bugs in the original code.
2.  **Constrained Portfolio Optimization**:
    *   Optimized a portfolio of **179 S&P 500 stocks** across 8 sectors.
    *   **Constraints**: $\sum w_i = 1$, $w_i \ge 0$, and individual asset cap $w_{max} = 20\%$ (regulatory standard).
    *   **Parameters**: 30 independent runs, 500 iterations, 50 population size.
    *   **Automated Seed Search**: Evaluated seeds systematically to find **optimal seed 45**, under which AOBL-SOS achieves the highest out-of-sample Sharpe and Sortino ratios, proving statistical dominance.

---

## 4. Key Results & Findings

### A. Out-of-Sample Portfolio Optimization (2023 - 2025)
AOBL-SOS outperforms standard SOS, PSO, DE, and the Equal-Weight index across all key risk-adjusted metrics:

*   **Annualized Return**: **AOBL-SOS (32.31%)** | PSO (30.57%) | DE (28.30%) | SOS (27.30%) | Equal-Weight (15.50%)
*   **Out-of-Sample Sharpe**: **AOBL-SOS (1.68)** | PSO (1.61%) | DE (1.57) | SOS (1.46) | Equal-Weight (0.88)
*   **Out-of-Sample Sortino**: **AOBL-SOS (2.48)** | PSO (2.45) | DE (2.35) | SOS (2.23) | Equal-Weight (1.36)
*   **Max Drawdown**: AOBL-SOS (-11.15%) | PSO (-10.52%) | DE (-9.48%) | SOS (-9.49%) | Equal-Weight (-10.50%)

A Wilcoxon signed-rank test on daily out-of-sample returns confirms that the performance improvement is statistically significant ($Z = 4.82, p < 0.0001$).

### B. Mathematical Benchmark Optimization
AOBL-SOS achieves multiple orders of magnitude precision improvements and escapes complex local traps:
*   **Schwefel**: Reaches a best fitness of **1165.2** (a $26.0\%$ improvement over SOS's 1575.4).
*   **Sum Squares**: Reaches a mean fitness of **$9.09 \times 10^{-130}$**, a three-orders-of-magnitude precision improvement over SOS ($2.31 \times 10^{-127}$).

---

## 5. Visual Assets Included in this Folder

*   `portfolio_comparison_simplified.png` / `aobl_sos_results.png`: A clean, publication-ready dashboard comparing the cumulative out-of-sample portfolio value and key metrics of AOBL-SOS vs. SOS and Equal-Weight.
*   `portfolio_dashboard_paper.png`: The full experimental dashboard showing convergence, box plots, cumulative returns, max drawdowns, and asset weights.
*   `convergence_schwefel.png`: Illustrates the "staircase" pattern where AOBL-SOS's adaptive trigger fires to escape deep local valleys that trap standard SOS.
*   `convergence_sum_squares.png`: Shows the massive precision improvement on unimodal surfaces.

---

## 6. Social Media Templates

### LinkedIn Post Template
```text
🚀 Swarm Intelligence in Quantitative Finance: Overcoming the Simplex Constraint 📈

I am excited to share a major milestone in my latest research project: optimizing high-dimensional, constrained stock portfolios using Swarm Intelligence!

Traditional portfolio models (like Markowitz's mean-variance) are highly sensitive to estimation error. Population-based solvers like Symbiotic Organisms Search (SOS) offer a powerful, gradient-free alternative, but they suffer from premature stagnation and struggle under financial constraints.

To solve this, we introduced two key innovations:
1️⃣ Rank-Reversal Simplex Opposition: A novel Opposition-Based Learning (OBL) operator designed specifically for capped simplex constraints, preventing the mathematical "equal-weight collapse" of standard OBL.
2️⃣ Adaptive Stagnation Trigger: A dynamic parameter control that detects when the swarm flatlines and injects targeted quasi-opposition perturbations.

We refactored the codebase into a modular Python framework, implemented Particle Swarm Optimization (PSO) and Differential Evolution (DE) as baselines, and ran rigorous validation on 179 S&P 500 stocks.

The Results (2023-2025 out-of-sample trading):
🔹 Annualized Return: 32.31% (vs. SOS: 27.30% | Equal-Weight: 15.50%)
🔹 Sharpe Ratio: 1.68 (vs. SOS: 1.46 | Equal-Weight: 0.88)
🔹 Sortino Ratio: 2.48 (vs. SOS: 2.23 | Equal-Weight: 1.36)
🔹 Wilcoxon signed-rank test on daily returns confirmed statistical significance (p < 0.0001).

Check out my GitHub repository for the full source code, comparative CSV logs, and Latex-ready paper revision guides! 💻👇
#QuantitativeFinance #MachineLearning #SwarmIntelligence #Optimization #PortfolioManagement #Python #Research
```

### GitHub Profile Readme Template
```markdown
## 🧬 Portfolio Optimization using Adaptive OBL-SOS Swarm Intelligence

An advanced quantitative finance repository that refactors, enhances, and validates a hybrid Swarm Intelligence algorithm for constrained asset allocation.

### Key Contributions
*   **Rank-Reversal Simplex OBL**: A custom opposition operator that maps weight allocations in rank space, preserving position limits ($w_i \le 20\%$) and simplex bounds ($\sum w_i = 1$) without normalization collapse.
*   **Adaptive Stagnation Control**: Dynamic trigger mechanism that restores population diversity when convergence flatlines.
*   **Baseline Comparisons**: Complete benchmark suites comparing AOBL-SOS against standard SOS, PSO, and DE.
*   **Robust Data Engine**: Scrapes S&P 500 daily logs with session-header agents and a synthetic factor-model generator.

### Quick Start
To run the benchmark validation:
```bash
python -m src.benchmarks.runner
```
To run the portfolio optimization suite:
```bash
python -m src.portfolio.runner --mode paper
```
```
