# Tomás Solano, InZenFenix, IIT414W, 20-04-2026

# C2: Reasoning & Interpretation

## 1. The Business Framing: Why F1-Score?

"The primary objective is to build a high-precision strategy tool. In F1 Qualifying, we are dealing with a binary classification where the 'cost' of a mistake is asymmetric.

I chose **F1-Macro** as the primary metric because it balances **Precision** (avoiding false optimism) and **Recall** (not missing a Top-10 threat). For a strategy team, predicting a slow car will make the Top 10 (**False Positive**) leads to wasted tire sets and compromised race setups. Conversely, missing a fast car (**False Negative**) leads to underestimating rivals. F1-Macro ensures the model performs equally well at identifying both 'Contenders' and 'Backmarkers' without being skewed by the 50/50 split of the grid."

## 2. Framing Decision: Why Classification?

"We chose **Classification** over Regression because the Head of Strategy needs a definitive threshold for decision-making (e.g., 'Do we send the driver out for a second run in Q2?'). While a regression on finishing position provides more detail, the cost of being wrong about P10 vs. P11 is catastrophic for points, whereas the cost of being wrong about P14 vs. P15 is negligible. Classification focuses the model's 'attention' on this critical point-scoring boundary."

| Model | Why this model? (Mechanistic Reasoning) | Behavior & Observations |
| :--- | :--- | :--- |
| **Logistic Regression** | Chosen for **linear transparency**. It serves as a baseline to see if lap-time deltas have a straight-line relationship with Top-10 probability. | **Robust but limited:** It struggled to capture "Non-Linearities" (e.g., a car fast on straights but failing in technical sectors). However, it had the lowest gap (-0.0074), proving most resistant to year-to-year shifts. |
| **Random Forest** | Chosen to capture **complex interactions** between specific teams and tracks (e.g., the 'Monaco effect' where chassis agility outweighs engine power). | **Overfitting Risk:** Showed a high Generalization Gap (0.0816). It overfit on 2024 hierarchies, failing to adapt quickly to 2025 'winter development' shifts. |
| **XGBoost** | Chosen for its **boosting architecture**, which iteratively corrects errors on 'marginal' drivers (those usually in P11-P13). | **Best Performer:** Achieved the highest Test F1 (0.9118). It proved most capable of distinguishing 'midfield' from 'top-tier' by weighting engineered features like `team_strength` effectively. |
| **Baselines** | Used to prove **Value-Add**. If the ML doesn't beat a 3-race rolling average, the complexity isn't justified. | **Confirmed Value:** The ML models significantly outperformed baselines in ROC-AUC, proving the engineered features provide a superior 'ranking' of driver talent over simple history. |

## 3. Metric Trade-off Analysis: F1 vs. ROC-AUC

"While F1-Score was the primary metric for selecting the best model, **ROC-AUC** was used as a secondary 'Stability Check.'

* **F1-Score** told us how many correct 'Top 10' labels we achieved for the 2025 season.
* **ROC-AUC** (which stayed high at 0.96-0.98) proved that even when the model missed the binary label, it still correctly **ranked** the drivers. For example, if the model predicted a driver as P11 when they got P10, the F1-Score penalizes it, but the ROC-AUC recognizes that the model correctly identified that driver as being on the 'edge' of the points, which is vital for risk-adjusted strategy."

## 4. Failure Mode Identification

"The most significant failure mode is the **'Season-Transition Blindspot.'** Because car performance is overhauled between years, models trained on 2024 data initially struggle with the 2025 technical pecking order. This is reflected in the Gap (Delta); the 'Calibrated' models are more aggressive and thus more prone to being 'surprised' by a team that suddenly improved their car over the winter (e.g., the 2025 Racing Bulls performance jump). To mitigate this, we rely on the Logistic Regression model during the first 3 races of a new season due to its superior robustness."