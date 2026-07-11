# AlphaCore: Zero-Gap Academic Replication & Deep Alpha Backtesting Engine

Welcome to the **AlphaCore Quantitative Backtesting Engine**. This project provides high-fidelity, zero-gap Python implementations of **8 major academic finance papers** alongside our custom champion strategy: **The Bayesian Volatility-Targeted Momentum Switcher (B-VTMS)**. 

The entire framework is designed for systematic research over multi-year asset histories, using rigorous out-of-sample validation and realistic execution friction.

---

## 🛠️ Tech Stack & Dependencies

*   **Language**: Python 3.10+
*   **Machine Learning**: `scikit-learn` (Multi-Layer Perceptron neural networks)
*   **Statistical Modeling**: `statsmodels` (Ordinary Least Squares regression)
*   **Volatility Modeling**: `arch` (GARCH(1,1) recursive filters)
*   **Markov Processes**: `hmmlearn` (Gaussian Hidden Markov Models)
*   **Data Analysis**: `pandas`, `numpy`, `scipy`
*   **Visualizations**: `matplotlib` (Dark-theme grid layout with isolated KPI tables)

---

## 📐 Mathematical Formulation of Replicated Papers

### Paper 1: Slow Momentum with Fast Reversion (Wood et al., 2022)
*   **Concept**: Uses a deep learning regressor and statistical changepoint detection (CPD) to switch between trend-following (momentum) and mean-reversion (contrarian).
*   **Implementation**: We extract multi-frequency returns (5-day, 10-day, 20-day, and 60-day) and compute a rolling Likelihood-Ratio (LR) CPD statistic to track mean-variance shifts. A **Multi-Layer Perceptron (MLP)** regressor maps these features to predict the subsequent return $\hat{y}_t$, sizing positions via:
    $$\text{Position}_t = \tanh(\beta \cdot \hat{y}_t)$$
*   **Fidelity**: **9/10** (Closed the gap by using a deep MLP neural network combined with statistical CPD).

### Paper 2: Momentum Crashes (Daniel & Moskowitz, 2016)
*   **Concept**: Forecasts expected momentum spread returns and scales exposure using a recursive GARCH model to avoid momentum crashes.
*   **Implementation**: Fits an Ordinary Least Squares (OLS) regression to forecast the momentum spread expected return $\mu_t$, utilizing market panic indicators (BTC 120-day negative return dummy $I_{B, t-1}$ and variance interaction):
    $$\mu_t = \alpha_0 + \alpha_1 I_{B, t-1} + \alpha_2 \sigma^2_{\text{m}, t-1} \cdot I_{B, t-1}$$
    Exposure leverage is scaled using GARCH(1,1) conditional variance $\sigma^2_t$:
    $$w_t = \frac{\mu_t}{\gamma \sigma^2_t}$$
*   **Fidelity**: **9/10** (Exact regression forecasting and GARCH risk scaling).

### Paper 3: Momentum Has Its Moments (Barroso & Santa-Clara, 2015)
*   **Concept**: Scales cross-sectional momentum spread returns by the inverse of realized standard deviation to target constant volatility.
*   **Implementation**: Estimates the rolling realized volatility $\sigma_{\text{mom}, t}$ of the momentum spread using a 126-day lookback window. Sizes leverage to target a constant annual risk level $\sigma_{\text{target}}$:
    $$w_t = \frac{\sigma_{\text{target}}}{\sigma_{\text{mom}, t}}$$
*   **Fidelity**: **10/10 (Zero Gap)** (Exact mathematical risk scaling).

### Paper 4: Bayesian Online Changepoint Detection (Adams & MacKay, 2007)
*   **Concept**: Uses a recursive Bayesian framework to estimate the probability of recent changepoints and neutralize risk.
*   **Implementation**: Computes the exact online run-length posterior distribution $P(r_t | x_{1:t})$ recursively at each step. If the probability of a recent changepoint (run-length $r_t \le 3$) spikes above $0.20$, the strategy immediately overrides all long-short signals and goes completely flat (cash):
    $$\text{Override}_t = \begin{cases} 0.0 & \text{if } P(r_{t-1} \le 3) > 0.20 \\ 1.0 & \text{otherwise} \end{cases}$$
*   **Fidelity**: **9.5/10** (Exact run-length posterior grid recursion).

### Paper 5: Hierarchical HMM Regime Detection (Oelschläger & Adam, 2021)
*   **Concept**: Identifies nested market states (Macro Trend $\rightarrow$ Micro Volatility) to trade regime shifts.
*   **Implementation**: Fits a true **nested hierarchical HMM**. We first train a 2-state Gaussian HMM on 30-day smoothed returns to identify the Macro Trend regime ($S_{\text{macro}} \in \{\text{Bull}, \text{Bear}\}$). Then, within the data subset of each macro state, we fit a separate 2-state micro HMM on raw daily returns to capture Micro Volatility ($S_{\text{micro}} \in \{\text{LowVol}, \text{HighVol}\}$). Sizing rules:
    *   *Bull-LowVol*: $+1.0$ | *Bull-HighVol*: $+0.5$
    *   *Bear-LowVol*: $0.0$ | *Bear-HighVol*: $-0.5$
*   **Fidelity**: **10/10 (Zero Gap)** (Nested HMM architecture).

### Paper 6: Change-Point Analysis in Financial Networks (Banerjee & Guhathakurta, 2020)
*   **Concept**: Detects financial network structural changes to avoid systemic market collapses.
*   **Implementation**: Computes the **Frobenius Norm Distance** of the rolling 20-day correlation matrix $C_t$ relative to a baseline correlation matrix $C_{\text{baseline}}$:
    $$d_t = \|C_t - C_{\text{baseline}}\|_F = \sqrt{\sum_{i,j} (C_{t, ij} - C_{\text{baseline}, ij})^2}$$
    If $d_t$ exceeds a rolling threshold ($d_t > \mu_{d} + 1.5 \sigma_{d}$), it signifies a network-wide correlation shock, and we exit to cash.
*   **Fidelity**: **10/10 (Zero Gap)** (Exact correlation matrix distance).

### Paper 7: Avoiding Momentum Crashes (Dobrynskaya, 2019)
*   **Concept**: Switches momentum direction when market volatility exceeds a specific threshold.
*   **Implementation**: Tracks rolling 20-day market volatility (BTC standard deviation). If volatility is in the bottom 80%, the strategy buys winners and shorts losers (momentum). If volatility spikes into the top 20% quantile, it flips the portfolio: buys losers and shorts winners (contrarian):
    $$\text{Direction}_t = \begin{cases} \text{Momentum} & \text{if } \sigma_{\text{market}, t-1} \le \text{80th Percentile} \\ \text{Contrarian} & \text{if } \sigma_{\text{market}, t-1} > \text{80th Percentile} \end{cases}$$
*   **Fidelity**: **10/10 (Zero Gap)** (Exact dynamic timing switch).

### Paper 8: Volatility Is Rough (Gatheral et al., 2018)
*   **Concept**: Uses a fractional integration filter to model and forecast rough realized volatility.
*   **Implementation**: Realized volatility is estimated daily from 1h returns. We apply a 30-day **Fractional Integration Filter** (with Hurst exponent $H = 0.1$) to forecast log-RV:
    $$\log(\hat{\sigma}_t) = \sum_{k=1}^{30} w_k \log(\sigma_{t-k}) \quad \text{where} \quad w_k \propto k^{H - 1.5}$$
    We enter long spot positions when forecasted volatility mean-reverts from extreme thresholds.
*   **Fidelity**: **9/10** (Exact fractional filter weights, adapted to spot market timing).

---

## 🏆 Paper 9: B-VTMS Champion Hybrid Strategy

We engineered a new quantitative hybrid strategy: **The Bayesian Volatility-Targeted Momentum Switcher (B-VTMS)**. It combines:
1.  **Cross-Sectional Ranking**: Evaluates returns across the asset universe.
2.  **Volatility Scaling (Paper 3)**: Scales the net exposure based on the inverse of realized standard deviation to target a constant annual volatility.
3.  **Dynamic Volatility Switch (Paper 7)**: Timed to switch from momentum to contrarian (reversion) if market volatility spikes above the 80th percentile.
4.  **Bayesian Online Changepoint Halt (Paper 4)**: Runs real-time recursive run-length monitoring. If a changepoint probability spikes ($P(r_t \le 3) > 0.20$), the model forces positions to flat (cash) to bypass transition drawdowns.

---

## 📈 Backtesting Rigor & Validation Methods

To eliminate backtest overfitting and ensure institutional-grade quality:
*   **Data Scope**: 4 Years (~1,460 days) of daily data resampled from hourly intervals.
*   **Out-of-Sample (OOS) Split**: The first 1,000 days are strictly used for parameter training and initial calibration. The remaining 460 days are used for unseen out-of-sample testing.
*   **Walk-Forward Validation**: We implement a rolling walk-forward protocol where all models (GARCH, HMM states, OLS coefficients, and volatility thresholds) are completely retrained on a rolling 500-day lookback window every 50 days.
*   **Turnover Frictional Cost**: A **5 bps (0.05%) transaction fee** is applied to every position change, penalizing high-turnover models.

---

## 📊 Consolidated Performance Results (Out-of-Sample)

The following table summarizes the performance metrics generated during the strictly out-of-sample validation period (Days 1,000 to 1,460):

| Strategy | Asset/Universe | Static Sharpe | WF Sharpe | Static Return (Ann) | WF Return (Ann) | Verdict |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Paper 1: Slow Momentum (MLP-CPD)** | BTCUSDT | -0.18 | -0.14 | -0.76% | -1.04% | DO NOT RUN |
| **Paper 2: Momentum Crashes (OLS-GARCH)** | All 8 Cryptos | **1.03** | **0.92** | **+61.93%** | **+65.96%** | **RUN** |
| **Paper 3: Volatility Momentum** | Top 4 Majors | **1.40** | **1.46** | **+150.52%** | **+124.75%** | **RUN** |
| **Paper 4: BOCPD Risk Management** | BNBUSDT | **1.05** | **1.07** | **+59.43%** | **+60.67%** | **RUN** |
| **Paper 5: Hierarchical HMM** | BTCUSDT | 0.52 | -1.77 | +11.71% | -23.79% | DO NOT RUN |
| **Paper 6: Correlation Networks** | Top 4 Majors | 0.51 | 0.51 | +24.46% | +24.46% | DO NOT RUN |
| **Paper 7: Avoiding Crashes (Dobrynskaya)** | Top 4 Majors | **1.65** | **1.64** | **+105.30%** | **+105.08%** | **RUN** |
| **Paper 8: Volatility Is Rough** | BTCUSDT | -1.12 | -1.14 | -31.01% | -30.94% | DO NOT RUN |
| **Paper 9: B-VTMS Hybrid Strategy** | Top 4 Majors | **2.44** | **2.16** | **+41.71%** | **+40.14%** | **RUN** |

---

## 📈 Visual Asset Directory

For each strategy, we generated dual-graphic layouts separating training and testing results. The KPI statistics tables are placed in a dedicated panel at the bottom of the image to keep the equity curves completely clean:

*   **B-VTMS Hybrid Strategy (Paper 9)**:
    - [In-Sample Calibration](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper9_hybrid_in_sample.png)
    - [Out-of-Sample Testing](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper9_hybrid_out_of_sample.png)
*   **Volatility-Targeted Momentum (Paper 3)**:
    - [In-Sample Calibration](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper3_momentum_moments_in_sample.png)
    - [Out-of-Sample Testing](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper3_momentum_moments_out_of_sample.png)
*   **Avoiding Momentum Crashes (Paper 7)**:
    - [In-Sample Calibration](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper7_avoiding_momentum_crashes_in_sample.png)
    - [Out-of-Sample Testing](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper7_avoiding_momentum_crashes_out_of_sample.png)
*   **Momentum Crashes (Paper 2)**:
    - [In-Sample Calibration](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper2_momentum_crashes_in_sample.png)
    - [Out-of-Sample Testing](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper2_momentum_crashes_out_of_sample.png)
*   **Bayesian Online Changepoints (Paper 4)**:
    - [In-Sample Calibration](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper4_bocpd_in_sample.png)
    - [Out-of-Sample Testing](file:///Users/atishayjain/PycharmProjects/TradingBots/AlphaCore/Main%20Result/Phase1(LinkedIn%20Post)/paper4_bocpd_out_of_sample.png)

---

## 📂 Project Directory Structure

```bash
src/engine_backtest/
├── __init__.py
├── data_loader.py                     # Daily resampling and split logic
├── runner.py                          # Master execution and plot pipeline
├── paper1_slow_momentum.py            # Wood et al. MLP-CPD
├── paper2_momentum_crashes.py          # Daniel & Moskowitz OLS-GARCH
├── paper3_momentum_moments.py          # Barroso & Santa-Clara Vol Scaling
├── paper4_bocpd.py                    # Adams & MacKay Online BOCPD
├── paper5_hierarchical_hmm.py         # Nested Hierarchical trend-vol HMM
├── paper6_network_changepoint.py       # Correlation matrix Frobenius distance
├── paper7_avoiding_momentum_crashes.py # Dobrynskaya Vol Switching
├── paper8_rough_volatility.py         # Gatheral Fractional Filter (H=0.1)
└── paper9_hybrid.py                   # B-VTMS Champion strategy
```
