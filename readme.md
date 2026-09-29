# Real-Time Credit Card Fraud Risk Engine

An end-to-end Machine Learning pipeline and operational decision architecture designed to detect credit card fraud under severe class imbalance. This project implements dynamic behavioral feature engineering, hyperparameter-tuned gradient boosting (LightGBM), and a 3-tier risk decision policy to maximize net savings while minimizing checkout friction.

---

## 🎯 Project Purpose
This project was developed as a hands-on **technical practice and case study** to address real-world financial risk trade-offs:
- Handling extreme class imbalance where standard evaluation metrics (e.g., Accuracy, ROC-AUC) fail.
- Moving beyond static demographic fields toward real-time behavioral velocity and spending anomalies.
- Aligning model probabilities directly with financial loss prevention and customer conversion metrics.

---

## 📊 Dataset Overview
The project evaluates multi-table relational credit card transaction data with temporal streaming characteristics:
- **Imbalance Ratio:** Base fraud rate of **~0.15%** (~1 fraud case per 667 legitimate transactions).
- **Data Scope:** 
  - Transaction logs (timestamps, transaction amounts, entry modes / Card-Not-Present flags).
  - Cardholder profiles and account metadata (credit limits, spending history, income, risk indicators).
  - Merchant categories (Merchant Category Codes / MCC descriptions).
- **Validation Strategy:** Chronological out-of-time splits to prevent temporal lookahead leakage and simulate real production streams.

---

## 🛠️ Tech Stack & Tools
- **Language:** Python 3.9+
- **Machine Learning:** LightGBM, Scikit-learn
- **Data Processing & Feature Engineering:** Pandas, NumPy
- **Evaluation & Financial Simulation:** Precision-Recall Curves (PR-AUC), Custom Cost-Benefit ROI Modeling
- **Visualization:** Matplotlib, Seaborn
- **Environment:** Jupyter Notebook

---

## ⚙️ Key Methodologies & Feature Engineering
1. **Behavioral Velocity Bursts:**
   - Rolling spend windows (`spend_ratio_1h_to_24h`) to capture rapid account-draining bursts.
   - Recency intervals (`time_since_prev_tx_sec`).
2. **Merchant & Category Deviations:**
   - Ratio of current spend relative to historical merchant category averages (`amount_to_mcc_avg_ratio`).
3. **Channel Exposure:**
   - High-value Card-Not-Present (CNP) e-commerce interaction flags.

---

## 📈 Benchmarks & Results
| Metric / Component | Baseline Model | Upgraded Pipeline | Impact |
| :--- | :--- | :--- | :--- |
| **Features Used** | Raw fields + Demographics | Behavioral Velocity + MCC Deviations | Behavioral dominance |
| **PR-AUC Score** | 0.0261 (~19× random lift) | **0.0445 (~31× random lift)** | **+54% Relative Lift** |
| **Net Economic Benefit** | — | **$48,534+** | Factoring OTP + friction costs |
| **Verification ROI** | — | **10.3 : 1** | $10.30 saved per $1.00 spent |
| **Legitimate Approval** | — | **99%+** | Near-zero customer friction |

---

## 🛡️ Operational Decision Architecture (3-Tier Policy)
To translate model probability scores into real-world business execution:
- **Tier 1: Auto-Approve (Score < 0.05):** Instantly authorizes ~97% of transactions with zero user friction.
- **Tier 2: Step-Up Authentication (Score 0.05 – 0.35):** Triggers dynamic 3DS / SMS OTP verification for borderline transactions.
- **Tier 3: Hard Decline (Score > 0.35):** Proactively blocks high-confidence attacks and halts account draining.

---
