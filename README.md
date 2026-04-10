# 📈 Behavioral Market Forecasting with Multi-Agent Reinforcement Learning (MARL)

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Machine Learning](https://img.shields.io/badge/Focus-Machine%20Learning-green.svg)]()
[![FinTech](https://img.shields.io/badge/Sector-FinTech-orange.svg)]()

> **⚠️ Technical Note:** To protect intellectual property and personal data, the core source code and the original internship report have been kept proprietary. This repository serves as a technical showcase of the methodology, behavioral heuristics, and simulation results.

---

## 🔍 Executive Summary
[cite_start]This project introduces a **behavioral finance framework** designed to predict the price direction of Solana (SOL) by modeling the complex interactions between institutional (Whale) and individual (Retail) market participants[cite: 440, 443]. [cite_start]By moving beyond traditional price-action models, the system identifies market manipulation and trend reversals through high-dimensional behavioral features and Reinforcement Learning[cite: 462, 498].

## 🧠 System Architecture

### 1. Behavioral Feature Engineering & ETL
The foundation of the model lies in its ability to segment market actors based on their footprint in the order book. [cite_start]The primary metric is the **Volume Ratio**, representing the average size of a single trade [cite: 443-447]:

$$Volume\_Ratio = \frac{\text{Volume}_t}{\text{Number of Trades}_t}$$

* [cite_start]**Participant Segmentation:** Activity is categorized using dynamic 10-day rolling quantiles ($Q$)[cite: 449]:
    * [cite_start]**Whales (Institutional):** Identified by higher volume-per-trade thresholds ($Q_{80}, Q_{70}, Q_{60}$)[cite: 450].
    * [cite_start]**Retail (Individual):** Identified by lower volume-per-trade thresholds ($Q_{20}, Q_{30}, Q_{40}$)[cite: 451].
* [cite_start]**Behavioral Indicators:** The ETL process generates features for **Accumulation** (buying pressure) and **Distribution** (selling pressure) for each segment, further refined to identify emotional extremes like **FOMO** and **Panic** [cite: 453-457].

### 2. Manipulation Analysis Heuristics
[cite_start]A custom heuristic module quantifies "unnatural" market movements by calculating a **Composite Manipulation Score** ($S_{composite}$) [cite: 499-501]:

$$S_{composite} = \alpha \cdot D + \beta \cdot L + \gamma \cdot H$$

Where:
* [cite_start]**Divergence ($D$):** Measures the net position disagreement between Whales and Retail traders[cite: 502, 504].
* [cite_start]**Dominance ($L$):** A logarithmic scale measuring the relative strength imbalance between groups [cite: 508-511].
* [cite_start]**Hold Gap ($H$):** Captures the total exposure difference in market participation [cite: 512-513].
* [cite_start]**Optimization:** Weighted at $\alpha=0.5, \beta=0.35, \gamma=0.15$ to prioritize distribution-phase traps [cite: 515-519].

### 3. The MARL Engine (Multi-Agent Reinforcement Learning)
[cite_start]The market environment is simulated using two independent Q-Learning agents [cite: 463-465, 468]:
* [cite_start]**Whale Agent:** Optimized to learn trend-setting strategies based on institutional capital flows[cite: 466, 480].
* [cite_start]**Retail Agent:** Optimized to model momentum-driven and sentiment-heavy public trading patterns[cite: 467].
* [cite_start]**Reward Mechanism:** Agents are governed by a **Relation Module**, where rewards are scaled based on an **Alignment Score**, encouraging the system to learn the strength of a trend through participant agreement [cite: 477, 481-483].
* [cite_start]**Adaptive Exploration:** An advanced performance-based $\epsilon$-decay strategy is implemented to ensure the agents escape local optima during volatile regimes without compromising learned stability [cite: 486, 488-494].

### 4. Final Prediction & Validation (XGBoost)
[cite_start]The final classification layer uses an **XGBoost Classifier** that aggregates MARL probability distributions with technical indicators [cite: 521, 528-531].
* [cite_start]**Validation:** 5-fold **TimeSeries Split** cross-validation was used to ensure zero look-ahead bias [cite: 532-533].

---

## 📊 Results & Performance

| Metric | Result | Technical Insight |
| :--- | :--- | :--- |
| **Final Test Accuracy** | **61.26%** | [cite_start]High predictive edge in volatile crypto markets [cite: 572-573]. |
| **ROC AUC Score** | **0.6728** | [cite_start]Significant discriminative capability[cite: 572, 591]. |
| **High Conf. Accuracy (Increase)** | **81.48%** | [cite_start]Achieved when increase probability $P > 0.7$[cite: 682, 694]. |
| **Risk Avoidance (Decrease Recall)** | **74.00%** | [cite_start]High sensitivity to downward trends for capital protection[cite: 622, 625]. |

### 🚀 Trading Simulation (1-Year Backtest)
Initial Capital: **$10,000** | [cite_start]Period: **Feb 2025 - Feb 2026** [cite: 718, 754-755].

* [cite_start]**High Confidence Strategy:** Reached **$62,926.89** (**+529.27% ROI**) [cite: 720, 738-739].
* [cite_start]**Buy & Hold Benchmark:** Ended at **$4,139.77** (**-58.60% Loss**) [cite: 725, 752-753].

---

## 🖼 Visual Evidence & Technical Analysis

### 📡 Agent Probabilities Heatmap
![Agent Probabilities Heatmap](./assets/agent_probabilities_heatmap.png)
*Technical Insight: This heatmap visualizes the action probabilities ($P$) generated by the MARL agents. It showcases how the Whale agent's high-confidence "Hold" or "Buy" signals often precede significant price shifts, while Retail volatility highlights market indecision.*

### 🛠 Feature Importance
![Feature Importance](./assets/feature_importance.png)
[cite_start]*Technical Insight: MARL-generated behavioral probabilities (Retail Sell/Buy Prob) dominate the model's decisions, ranking significantly higher than traditional indicators like Bollinger Bands or RSI [cite: 575-581, 647-655].*

### 📉 Confusion Matrix
![Confusion Matrix](./assets/confusion_matrix.png)
*Technical Insight: The matrix demonstrates the model's 74% recall for downward movements, proving its efficacy as a risk management tool in bear markets.*

### 📅 Performance Distribution
![Monthly and Annual Performance Analysis](./assets/monthly_annual_performance_analysis.png)
[cite_start]*Technical Insight: Monthly accuracy analysis reveals the model's adaptability across different market regimes, peaking at 80% accuracy during high-trend months [cite: 696-698].*

### 💰 Capital Growth Comparison
![Capital Growth](./assets/capital_growth.png)
[cite_start]*Technical Insight: A 365-day equity curve comparison showing the "High Confidence" strategy vastly outperforming the asset's raw performance during a major market decline [cite: 717-725, 756].*

### 📊 Detailed Simulation Metrics
![Trading Simulation Results](./assets/trading_simulation_results.png)
[cite_start]*Technical Insight: Comprehensive breakdown of trade counts, profit/loss days, and net ROI after adjusting for potential transaction costs [cite: 726-728, 730-756].*

---

## ⚙️ Requirements
To replicate the environment or run the analytical scripts:
```text
pandas
numpy
xgboost
scikit-learn
pandas_ta
matplotlib
seaborn
requests
---

##Contact
For a detailed technical walkthrough or to discuss the findings, feel free to reach out via **[LinkedIn](https://www.linkedin.com/in/huseyinasimferik)**.
