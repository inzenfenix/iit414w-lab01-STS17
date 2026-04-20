# Tomás Solano, InZenFenix, IIT414W, 20-04-2026
 

 # PROMPTS.md


## 1. Feature Engineering: Beyond the Basics
> **Context:** Looking for domain-specific features that could improve prediction without causing target leakage.

> **Prompt:**
> ```text
> What other features could i use from F1 Datasets (both fastf1 and jolpica api) to predict if someone gets top 10 on a race? (Preventing leakage, and how can i get at least 3 of this features onto a pandas dataframe)
> ```
> **Relevant Output:**
> Proposed using "Team Strength" (constructor-wide averages), "Driver Form" (rolling last 3 races), and "Sector-Specific Performance" to capture car-track suitability.

> **Final Decision:** Used

> **Validation:** Provided the foundation for the `team_strength` and `driver_form` columns seen in the final dataframe.

> **Adaptations:** These features were implemented using rolling averages and shift operations to prevent temporal leakage.

---

## 2. The Identity Bias vs. Data Form Trade-off
> **Context:** Deciding whether to use the driver's name as a categorical feature in the model.

> **Prompt:**
> ```text
> should i use the name? i mean if a driver is known to be good they usually win right?
> ```

> **Relevant Output:**
> Highlighted the risk of "Identity Bias," where a model memorizes a name rather than learning the statistics of performance. Recommended using "Driver Form" instead to allow the model to generalize to new rookies.

> **Final Decision:** Used

> **Validation:** This decision was critical for the 2025 Test set, ensuring the model looked at the car/driver performance rather than just 2024 results.

> **Adaptations:** Excluded `FullName` from the final training features.

---

## 3. Modeling Non-Linear Relationships
> **Context:** Choosing between simple linear models and complex ensemble methods.

> **Prompt:**
> ```text
> Let's create the 3 models and get the f1-scores, what would be the best way to create this pipeline?
> ```

> **Relevant Output:**
> Discussion on how XGBoost and Random Forest can capture non-linear relationships (like a car that is fast on straights but fails in technical sectors) that Logistic Regression might miss.

> **Final Decision:** Used

> **Validation:** Confirmed by the final results where XGBoost outperformed the linear baseline in Test F1-Macro.

> **Adaptations:** All three models were kept to demonstrate the trade-off between complexity and robustness.

---

## 4. Temporal Data Splitting Logic
> **Context:** Structuring the train/test split specifically for a sports-prediction use case.

> **Prompt:**
> ```text
> Created the scaled data, can you validate this is a valid way of scaling data? Or is there any other option: <Python Code>
> ```

> **Relevant Output:**
> Verified that a temporal split (2024 for training, 2025 for testing) is the only valid way to simulate real-world prediction, as it prevents "future data" from leaking into the training phase.

> **Final Decision:** Used

> **Validation:** This split revealed the "Generalization Gap" caused by inter-season car development.

> **Adaptations:** Standardized the split across all model evaluations for fair comparison.

---

## 5. Defining the Heuristic Baseline
> **Context:** Establishing a "Constructor Tier" baseline to see if the AI adds more value than just knowing which team is fastest.

> **Prompt:**
> ```text
> Can you give me the metrics with the train set? Want to compare that to my test set's metrics, also Can you append it to the dataframe for display purposes? (including baseline)
> ```

> **Relevant Output:**
> Assisted in defining the "Constructor Tier" baseline using expanding windows to ensure the heuristic also respected the temporal firewall.

> **Final Decision:** Used

> **Validation:** Showed that while the Constructor Tier is a strong baseline, ML models like XGBoost provide a significant boost in ROC-AUC.

> **Adaptations:** Integrated the heuristic results into the final `full_comparison_df`.

---

## 6. Probability Calibration for Decision Making
> **Context:** Analyzing the output of Logistic Regression vs. Tree-based models for strategy calls.

> **Prompt:**
> ```text
> Does logistic regression gets calibrated inmediately when you fit it? Compared to the other models, i mean
> ```

> **Relevant Output:**
> Detailed explanation on how Logistic Regression produces naturally calibrated probabilities, while XGBoost and RF require post-processing to be "honest" about their confidence levels.

> **Final Decision:** Used

> **Validation:** Influenced the decision to report "Calibrated" versions of the ensemble models in the final table.

> **Adaptations:** Used `CalibratedClassifierCV` to ensure the strategy probabilities were reliable for the C3 Memo.

---

## 7. Analyzing the Generalization Gap
> **Context:** Determining the impact of the "Winter Break" on model accuracy.

> **Prompt:**
> ```text
> how can i get the delta (gap) between train and test?
> ```

> **Relevant Output:**
> Methodology for calculating the difference between 2024 performance and 2025 performance. This identified that car development shifts lead to a drop in F1-Score as the model "lags" behind current season reality.

> **Final Decision:** Used

> **Validation:** Directly provided the data for the C2 reasoning section regarding "Season Transition Volatility".

> **Adaptations:** Added the `Gap (Delta)` column to the final presentation artifacts.
