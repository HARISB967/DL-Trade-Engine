# 📈 Regime-Aware Algorithmic Trading with Regularized LSTMs

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)
![Pandas](https://img.shields.io/badge/Pandas-Data_Processing-green.svg)
![Finance](https://img.shields.io/badge/Finance-Algorithmic_Trading-lightgrey.svg)

## 📌 Project Overview
This project bridges the gap between theoretical deep learning and practical quantitative finance. It implements a robust, regime-aware algorithmic trading system capable of generating consistent alpha while strictly mitigating common quantitative pitfalls such as data leakage, severe overfitting, and concept drift.

Tested on **Banco Bradesco (BBD)**, the architecture transitions from a highly regularized baseline LSTM into an advanced execution framework utilizing **Dynamic Average True Range (ATR) Trailing Stops** and **Walk-Forward Online Learning**.

## ✨ Key Features
* **Stationary Feature Engineering:** Converts raw prices into momentum-based stationary indicators (Distance from SMAs, Normalized ATR, RSI) to prevent the neural network from memorizing historical price ticks.
* **Macro-Regime Awareness:** Integrates a `SMA_200_Slope` feature to explicitly inform the AI of structural Bull vs. Bear market regimes.
* **Heavy LSTM Regularization:** Utilizes severe network constraints (L2 weight decay, 0.4 Dropout, Batch Normalization) to deliberately lower training accuracy to ~72%, forcing the model to learn generalized concepts rather than memorizing noise.
* **Dynamic Volatility Risk Management:** Replaces fragile static stop-losses with a Dynamic ATR Trailing Stop (`ATR * 2.0`) that automatically widens during high volatility and tightens during market consolidation.
* **Walk-Forward Optimization (Online Learning):** Combats *Concept Drift* by pausing execution every 60 days to retrain the neural network on the trailing 252 days, completely neutralizing the "perma-bear" bias common in static models.

## 🏗️ System Architecture

1. **Data Pipeline:** Ingests OHLCV data, cleans artifacts, engineers stationary features, and generates asymmetric barrier targets (Hold/Buy/Sell).
2. **Model Training (Phase 1):** Trains the regularized Multi-Class LSTM, utilizing class weights to balance the label distribution.
3. **Execution Layer (Phase 2):** Maps model probabilities (`argmax`) to entry signals and applies the Take-Profit/Dynamic ATR stop-loss hierarchy.
4. **Out-of-Sample Inference (Phase 3):** Tests the strategy on completely unseen future data using either strict static inference or walk-forward optimization, exporting a clean execution log to Excel.

## 📊 Performance & Results

Tested on 3 years of unseen, out-of-sample market data (2023–2026).

### 1. Strict Static Inference (Zero Retraining)
Tested using a frozen model to prove viability without rolling optimization or test-set peeking.
* **Win Rate:** 60.0%
* **Average Holding Period:** 16.3 days
* **Benchmark Outperformance:** **+44.67%** excess return (vs. 4% fixed-yield baseline)

### 2. Walk-Forward Optimization (Advanced Extension)
Model weights dynamically updated every 60 days to adapt to shifting market regimes.
* **Win Rate:** 65.0%
* **Average Holding Period:** 11.5 days
* **Benchmark Outperformance:** **+52.68%** excess return (vs. 4% fixed-yield baseline)

## 🚀 Installation & Usage

**1. Clone the repository:**
```bash
git clone [https://github.com/YourUsername/Regime-Aware-Trading-LSTM.git](https://github.com/YourUsername/Regime-Aware-Trading-LSTM.git)
cd Regime-Aware-Trading-LSTM
