# Model Exploration Log: F1 Top-10 Prediction

## 1. Candidate Model: Logistic Regression
* **Suitability:** High (Baseline).
* **Theoretical Strength:** F1 qualifying results often have a linear relationship with lap time deltas. Logistic Regression provides a clear, calibrated probability which is essential for risk-management on the pit wall.
* **Expected Behavior:** Low variance, but potentially high bias. It may struggle to capture complex "team + track" interactions (e.g., a car that is only fast in low-speed corners).
* **Decision:** **Selected** as the primary linear baseline.

---

## 2. Candidate Model: Random Forest (Ensemble)
* **Suitability:** Medium-High.
* **Theoretical Strength:** Since F1 data has many categorical variables (Track names, Team names), tree-based models are excellent because they don't require feature scaling to work and can capture non-linear "rules" (e.g., If Team=Ferrari AND Track=Monza, then Performance=High).
* **Expected Behavior:** Likely to provide high accuracy but prone to "memorizing" the 2024 season (overfitting).
* **Decision:** **Selected** to test the impact of ensemble learning on categorical data.

---

## 3. Candidate Model: XGBoost (Gradient Boosting)
* **Suitability:** Very High.
* **Theoretical Strength:** F1 is a sport of "marginal gains." XGBoost's iterative approach—where each new tree corrects the errors of the previous ones—is perfect for distinguishing between P10 (Target) and P11 (Fail), which are often separated by milliseconds.
* **Expected Behavior:** Highest predictive power but requires careful calibration to ensure the probabilities aren't overly "confident."
* **Decision:** **Selected** as the primary high-performance candidate.

---

## 4. Candidate Model: Support Vector Machines (SVM)
* **Suitability:** Low.
* **Theoretical Rationale:** While good for high-dimensional data, SVMs are computationally expensive on larger datasets and do not naturally provide probability estimates without using "Platt Scaling," which adds complexity for little gain over Logistic Regression in this specific domain.
* **Decision:** **Rejected** in favor of Logistic Regression for better interpretability and speed.

---

## 5. Candidate Model: K-Nearest Neighbors (KNN)
* **Suitability:** Low.
* **Theoretical Rationale:** KNN relies on "distance" between points. In F1, "distance" is hard to define—is a driver "closer" to another driver because they are on the same team, or because they have similar lap times? The lack of a clear distance metric for categorical F1 features makes this a poor fit.
* **Decision:** **Rejected** due to poor handling of categorical variables and lack of probabilistic output.

---

## Summary of Final Selection
We will move forward with **Logistic Regression** (Linear/Calibrated), **Random Forest** (Non-linear/Bagging), and **XGBoost** (Non-linear/Boosting). This trio allows us to compare three fundamentally different mathematical approaches to the same F1 strategy problem.