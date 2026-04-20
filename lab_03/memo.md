# Tomás Solano, InZenFenix, IIT414W, 20-04-2026

# C3: Memo to Head of Strategy

---

**To:** Head of Race Strategy  
**From:** Lead Data Scientist  
**Date:** April 20, 2026  

---

## Subject: Selection of Predictive Model for Top-10 Qualifying Probability

### 1. Executive Summary
Following an extensive evaluation of five predictive frameworks using 2024 historical data and 2025 "live" test results, I recommend the implementation of the **Calibrated XGBoost** model for our qualifying strategy operations. This model provides a superior balance of ranking accuracy (**ROC-AUC: 0.9680**) and classification performance (**F1-Macro: 0.9118**) compared to our existing rolling-average heuristics.

### 2. Model Performance & Selection Logic
While three machine-learning architectures were tested, the **Calibrated XGBoost** emerged as the optimal choice for the following reasons:
* **Superior Competitive Ranking:** The model’s high ROC-AUC proves it can accurately rank the field from P1 to P20. Even if car performance shifts globally, the model maintains a high "order accuracy," allowing us to identify which rivals are the true threats for Q3 entry.
* **Value-Add Over Baselines:** The model significantly outperformed the Driver Rolling Average (**F1: 0.7689**), proving that our integration of `team_strength` and `avg_delta_season` captures car-specific performance that simple driver history misses.
* **Predictive Precision:** The model achieved a Test F1-Score of **0.9118**, meaning it is highly reliable in a binary "In/Out" scenario for the Top 10.

### 3. Identified Risks & Mitigation
Our analysis identified a specific failure mode regarding **"Season Transition Volatility."**
* **The Problem:** All models showed a performance drop (Gap) between 2024 and 2025. This is due to "Winter Development," where teams like Racing Bulls made significant jumps that historical data could not anticipate.
* **The Mitigation:** During the first three races of a new season, we should apply a **"Stability Buffer."** We will rely more heavily on the **Logistic Regression** outputs during these transition periods, as it demonstrated the lowest Generalization Gap (**-0.0074**) and higher robustness to technical shifts.

### 4. Strategic Recommendation
For the upcoming Grand Prix, the pit wall should utilize the Calibrated XGBoost probabilities to determine tire allocation for Q1 and Q2.

* **Thresholding:** If the model provides a probability **>85%**, we recommend a single-run strategy to save soft tires for Q3.
* **Risk Alert:** For probabilities between **40%−60%**, the strategy should default to an "Aggressive Defense," as these drivers represent the highest risk of a Q2 exit based on current midfield volatility.