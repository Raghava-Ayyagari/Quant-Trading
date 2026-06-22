# Momentum Change  Predictive Trading Strategy

A basic, from-scratch quantitative trading strategy . This project implements a multi-model look-forward predictive matrix that balances short-term market momentum against an ensemble of long-term structural trends. The strategy operates without dependency on high-level machine learning frameworks, relying entirely on **NumPy** and **Pandas** for vectorized matrix calculations, volatility normalization, and execution tracking.

---

## Introduction & Experimental Evolution

This project began as an entry-level exploration into quant, focusing on a single asset to predict its future directional position from basic probability principles. Early optimization passes revealed that the strategy could be aggressively tuned to individual historical datasets by identifying the exact long-term lookback remainder ($T_l \pmod{30}$) that maximized backtested risk-adjusted returns. 

However, forward out-of-sample validation exposed a classic quantitative obstacle: a remarkably weak correlation between the optimal parameters found on historical data and their actual performance on future live data. 

To prevent overfitting and tame execution volatility, the strategy overhauled into a robust ensemble framework. By securing a structurally stable short lookback window ($T_s$) and evaluating a diverse spectrum of concurrent long lookback windows ($T_l$) across an array of shifting look-forward time horizons ($t$), the system gained structural stability through model diversification.

### Current Performance Profile & Horizons
Multi-stock backtesting reveals highly regime-dependent performance profiles:
* **Asset Specificity:** The model delivers excellent, sustained risk-adjusted returns on specific equity structures, but encounters structural friction and fails on others where underlying structural drift assumptions break down.
* **Shorting Mechanics (-1):** While the current baseline operates on a long-only fractional scale [0.0, 1.0], experimental adjustments enabling two-sided short positions (-1) completely alter the strategy's equity curve topology. Allowing short entries sometimes dilutes alpha on historically optimal trending stocks, but significantly stabilizes and rehabilitates performance on previously failing, mean-reverting tickers.

Ultimately, this project serves as a deeply insightful mathematical experiment. While the predictive mechanics still possess significant scope for parameter refinement and structural optimization, building this system from the ground up provided invaluable practical exposure to data leakage prevention, statistical resonance, and vectorization design.

---

##  Strategy Architecture & Detailed Mathematical Derivation

The code attached uses average of difference weighted by lookback time to generate a signal using a formula.The derivation of the formula and conditions arise from some assumptions about the market and some basic principles of probability ,sepcifically estimation and concentration inequalitites.
The mathematical formulation and code explanation is detailed in another document.Following will be an analysis of the original strategy followed by the changes made to make is less volatile.Visualisations are available in the attached notebook and also in the pictures that would be attached.

## 📊 Empirical Analysis & Parameter Topology

### 1. Cyclostationary Parameter Resonance (Modulo Stride)
A deeper exploration of the parameter landscape revealed an architectural phenomenon: **cyclostationary parameter resonance** driven by the strategy's update stride interval (**T = 30** minutes).

By holding the short lookback window ($T_s$) static and systematically permuting the long lookback window ($T_l$) across wide integer ranges, the resulting annualized Sharpe Ratios were tracked, mapped, and grouped. All $T_l$ configuration paths sharing a matching remainder when divided by the stride interval ($T_l \pmod{30}$) create tightly clustered, stratified risk-adjusted return bands. Empirically, **higher modulo remainders yield significantly elevated Sharpe ratio profiles**, indicating optimized phase alignment with the structural market rebalancing cycles.

### 2. Signal Activation Topology (Sigmoid vs. Signum)
The structural formation of these parameter bands depends heavily on the mathematical activation functions used to scale raw directional conviction:

* **Continuous Sigmoid Transformation ($f(x) = \frac{1}{1 + e^{-x}}$):** Compresses variance across phase offsets. By smoothing the gradient boundaries of underlying model transitions, the performance profiles of differing modulo remainder cohorts are pulled closely together into a dense, upward-trending performance channel.
* **Discrete Signum Transformation ( $f(x) = \text{sgn}(x)$ ):** Strips away continuous gradient scaling and isolates raw binary directional conviction. This choice amplifies the structural phase delays among parameter pairs, splitting the parameter resonance wide open into independent, horizontally stratified performance bands.

### 3. Log-Space Momentum Transition Distribution
Tracking the temporal properties of the strategy reveals that the intervals between predicted trend transition turning points ($\Delta \tau = \tau_{k+1} - \tau_k$) follow a highly specific geometric distribution.

When the transformation is mapped into natural log space $\ln(\Delta \tau)$, the probability density function (PDF) resolves into a **Jonhson Distribution** whose parameters can be observed:

$$f(x) = \frac{\delta}{\lambda \sqrt{2\pi}} \frac{1}{\sqrt{1 + \left(\frac{x-\xi}{\lambda}\right)^2}} \exp\left[ -\frac{1}{2} \left( \gamma + \delta \sinh^{-1}\left(\frac{x-\xi}{\lambda} \right) \right)^2 \right]$$

> **Structural Insight:** The clean  profile in log space proves that market regime durations are fundamentally asymmetric. The sharp, bounded left tail establishes a clear physical limit on minimum regime velocity (preventing rapid, high-frequency signal whip-sawing). Conversely, the elongated, heavy right tail mathematically accounts for rare, highly stable macroeconomic trends that persist exponentially longer than traditional symmetric gaussian models assume.

---

## Further scope of analysis
Further analysis can be done as to why the time series follows such a distribution or why the sharpe ratio follows such a cyclic trend.The period of 30 itself might be dependent heavily on rest of the parameters like short window ,threshold etc.A mathematical derivation aside from the intuitive explanation is needed.

---
## Conclusion
This was a beginer project where I had great fun devising  a strategy mathematically , implementing it and devising modifications to strategy based on intial testing ,avoiding any form of look ahead bias or data leakage.
The starting versions of code were written from scratch , and vectorised versions which followed the same strategy but greatly optimised time complexity were vibecoded .
Overall I had great fun developing it and learned some new things.I hope to develop much more in the world of quantative trading applying more complex strategies (using more features ,multiple assets etc).
