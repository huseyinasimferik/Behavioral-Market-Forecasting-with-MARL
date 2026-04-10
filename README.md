# Behavioral Market Forecasting with Multi-Agent Reinforcement Learning (MARL)

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Machine Learning](https://img.shields.io/badge/Focus-Machine%20Learning-green.svg)]()
[![FinTech](https://img.shields.io/badge/Sector-FinTech-orange.svg)]()

> **Technical Note:** To protect intellectual property and personal data, I have kept the core source code and the original internship report proprietary. This repository serves as my technical portfolio, showcasing the architecture, behavioral heuristics, and simulation results I developed during my internship at Sade Software and Consulting.

---

## Executive Summary
In this project, I developed a **behavioral finance framework** to predict the price direction of Solana (SOL). My approach focuses on modeling the interactions between different market participants—specifically "Whales" (Institutional) and "Retail" (Individual) traders. Unlike traditional models that rely only on price patterns, I designed this system to detect market manipulation and trend reversals by identifying the volumetric footprints of these actors. As a student at METU, this project allowed me to combine academic rigor with real-world financial data science.

## System Architecture

### 1. Behavioral Feature Engineering & ETL
I built the foundation of my model on the **Volume Ratio** metric. I used this to filter market noise and identify who is leading the price action at any given time. This ratio represents the average size of a single trade:

$$Volume\_Ratio = \frac{\text{Volume}_t}{\text{Number of Trades}_t}$$

* **My Segmentation Logic:** I categorized market activity using dynamic 10-day rolling quantiles ($Q$). This ensures that my definition of a "Whale" adapts to changing market liquidity.
    * **Whales:** Identified by the highest volume-per-trade thresholds ($Q_{80}, Q_{70}, Q_{60}$).
    * **Retail:** Identified by lower thresholds ($Q_{20}, Q_{30}, Q_{40}$), representing smaller, more frequent trades.
* **Sentiment Modeling:** I created features to track **Accumulation** (buying) and **Distribution** (selling) for each group. I also implemented indicators to detect emotional extremes, which I labeled as **FOMO** and **Panic**.

### 2. Manipulation Analysis Heuristics
I developed a unique heuristic module to quantify "unnatural" market movements. I formulated a **Composite Manipulation Score** ($S_{composite}$) to identify potential market traps:

$$S_{composite} = \alpha \cdot D + \beta \cdot L + \gamma \cdot H$$

* **Divergence ($D$):** I measure the disagreement between Whale and Retail positions.
* **Dominance ($L$):** I use a logarithmic scale to measure the relative strength of Whales over Retail.
* **Hold Gap ($H$):** I capture the total exposure difference between the two groups.
* **Optimization:** I weighted these as $\alpha=0.5, \beta=0.35, \gamma=0.15$. By giving the most weight to Divergence, I improved the model's ability to spot distribution phases before a price drop.

### 3. The MARL Engine (Multi-Agent Reinforcement Learning)
The core of my system is a Multi-Agent environment where I trained two independent Q-Learning agents:
* **Whale Agent:** I optimized this agent to learn institutional strategies and trend-setting behaviors.
* **Retail Agent:** I designed this to model sentiment-driven and momentum-heavy patterns.
* **Alignment Reward:** I implemented a **Relation Module** where agents receive higher rewards if their actions align. This helps the system confirm the strength of a trend through participant agreement.
* **Adaptive Exploration:** I created an advanced $\epsilon$-decay strategy. If the model stops making learning progress, it automatically increases exploration to find better strategies in volatile regimes.

### 4. Final Predictive Model (XGBoost)
I used an **XGBoost Classifier** as the final decision layer. It aggregates the probabilities from my MARL agents with technical indicators. To ensure my results were honest and robust, I used a 5-fold **TimeSeries Split**, which prevents the model from "seeing" future data during training.

---

## Results & Performance

| Metric | Result | My Technical Insight |
| :--- | :--- | :--- |
| **Final Test Accuracy** | **61.26%** | This is a significant "predictive edge" in a market as volatile as crypto. |
| **ROC AUC Score** | **0.6728** | Confirms the model's strong ability to distinguish between up and down moves. |
| **High Conf. Accuracy** | **81.48%** | Success rate when the model is very confident ($P > 0.7$). |
| **Risk Avoidance (Recall)** | **74.00%** | The model is exceptionally good at identifying and avoiding downward trends. |

### Trading Simulation (1-Year Backtest)
I started the simulation with **$10,000**.
* **My High Confidence Strategy:** Reached **$62,926.89** (**+529.27% ROI**).
* **Buy & Hold Benchmark:** During the same period, simply holding SOL resulted in a **$4,139.77** final balance (**-58.60% Loss**).

---

## Visual Evidence & Detailed Analysis

### Agent Probabilities Heatmap
![Agent Probabilities Heatmap](./assets/agent_probabilities_heatmap.png)
**My Analysis:** This heatmap is a window into the "minds" of my MARL agents. You can see how the Whale agent's high-confidence signals often act as a leading indicator. When the heatmap shows Whale accumulation while Retail remains indecisive, it typically precedes a healthy trend.

### Feature Importance
![Feature Importance](./assets/feature_importance.png)
**My Analysis:** I found it fascinating that the behavioral probabilities I generated (Retail Sell/Buy Prob) completely dominate the model's decision-making. They rank much higher than standard indicators like RSI or Bollinger Bands. This proves my thesis: in crypto, understanding *who* is trading is more important than just looking at the price.

### Confusion Matrix
![Confusion Matrix](./assets/confusion_matrix.png)
**My Analysis:** This is the most important chart for risk management. My model successfully caught 74% of all downward movements. For a trader, this means the system is not just a profit generator, but a powerful shield that tells you when to move to cash.

### Performance Distribution
![Monthly and Annual Performance Analysis](./assets/monthly_annual_performance_analysis.png)
**My Analysis:** I analyzed the model's success month-by-month to see how it handles different market "regimes." While accuracy fluctuates, it peaked at 80%, showing that the system excels when there is a clear behavioral trend to follow.

### Capital Growth Comparison
![Capital Growth](./assets/capital_growth.png)
**My Analysis:** This equity curve is the ultimate proof of my work. While the market (orange line) crashed by nearly 60%, my High Confidence strategy (green line) grew the capital over 6 times. It shows that behavioral modeling can find "Alpha" even in the middle of a bear market.

### Detailed Simulation Metrics
![Trading Simulation Results](./assets/trading_simulation_results.png)
**My Analysis:** Here I break down the trade counts and profit/loss days. Even after accounting for potential Binance fees and slippage, the strategy remains highly profitable. It validates that my model produces actionable and sustainable trading signals.

---

## Requirements
To set up the environment I used for data processing and modeling:
```text
pandas
numpy
xgboost
scikit-learn
pandas_ta
matplotlib
seaborn
requests
```

---

## Contact
For a detailed technical walkthrough or to discuss the findings, feel free to reach out via **[LinkedIn](https://www.linkedin.com/in/huseyinasimferik)** or **[Gmail](mailto:huseyinasimferik@gmail.com)**.
