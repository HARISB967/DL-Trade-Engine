# 📈 Regime-Aware Algorithmic Trading with Regularized LSTMs

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)
![Pandas](https://img.shields.io/badge/Pandas-Data_Processing-green.svg)
![Finance](https://img.shields.io/badge/Finance-Algorithmic_Trading-lightgrey.svg)

## 📌 Project Overview
This project bridges the gap between theoretical deep learning and practical quantitative finance. It implements a robust, regime-aware algorithmic trading system capable of generating consistent alpha while strictly mitigating common quantitative pitfalls such as data leakage, severe overfitting, and concept drift.

Tested on **Banco Bradesco (BBD)**, the architecture utilizes a highly regularized Long Short-Term Memory (LSTM) neural network paired with an advanced execution framework featuring **Dynamic Average True Range (ATR) Trailing Stops**. To ensure strict academic and quantitative compliance, the system is evaluated using zero-leakage static inference.

## ✨ Key Features
* **Stationary Feature Engineering:** Converts raw prices into momentum-based stationary indicators (Distance from SMAs, Normalized ATR, RSI) to prevent the neural network from memorizing historical price ticks.
* **Macro-Regime Awareness:** Integrates a `SMA_200_Slope` feature to explicitly inform the AI of structural Bull vs. Bear market regimes.
* **Heavy LSTM Regularization:** Utilizes severe network constraints (L2 weight decay, 0.4 Dropout, Batch Normalization) to deliberately lower training accuracy to ~72%, forcing the model to learn generalized concepts rather than memorizing noise.
* **Dynamic Volatility Risk Management:** Replaces fragile static stop-losses with a Dynamic ATR Trailing Stop (`ATR * 2.0`) that automatically widens during high volatility and tightens during market consolidation.
* **Strict Static Inference (Zero Data Leakage):** The model weights are completely frozen after the training phase. Predictions on the 3-year unseen test set are generated without any rolling optimization, retraining, or look-ahead bias, proving the true out-of-sample robustness of the architecture.

## 🏗️ System Architecture

1. **Data Pipeline:** Ingests OHLCV data, cleans artifacts, engineers stationary features, and generates asymmetric barrier targets (Hold/Buy/Sell).
2. **Model Training (Phase 1):** Trains the regularized Multi-Class LSTM, utilizing class weights to balance the label distribution.
3. **Execution Layer (Phase 2):** Maps model probabilities (`argmax`) to entry signals and applies the Take-Profit/Dynamic ATR stop-loss hierarchy.
4. **Out-of-Sample Inference (Phase 3):** Tests the strategy on completely unseen future data using a strict static prediction pass, exporting a clean execution log to Excel.

## 📊 Performance & Results

The system was evaluated on a 3-year unseen, out-of-sample market dataset (2023–2026). Because the model was frozen, the architecture successfully relied on its dynamic risk management layer to survive regime shifts and secure profits.

### Final Unseen Test (Static Inference)
* **Final Portfolio Value:** $4,740.41
* **Win Rate:** 60.0% (across 15 executed trades)
* **Average Revenue per Trade:** $102.87
* **Average Holding Period:** 16.3 days
* **Benchmark Outperformance:** **+44.67%** excess return (vs. 4% fixed-yield baseline)

## 🚀 Installation & Usage

**1. Clone the repository:**
```bash
git clone [https://github.com/YourUsername/Regime-Aware-Trading-LSTM.git](https://github.com/YourUsername/Regime-Aware-Trading-LSTM.git)
cd Regime-Aware-Trading-LSTM
