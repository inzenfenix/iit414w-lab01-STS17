# COMPARISON TABLE

## COMPARES ALL THE FINAL MODELS WITH THE BASELINE ON BOTH TRAIN AND TEST SET METRICS

## MAIN METRIC: F1-SCORE

### Target: Will the Driver get a Top 10 position by the end of the race? (Classification)

| Model                              | Train F1 (2024) | Test F1 (2025) | F1 Gap (Delta) | Test Accuracy | Test ROC-AUC |
| :--------------------------------- | :-------------: | :------------: | :------------: | :-----------: | :----------: |
| **Calibrated XGB**                 |     0.9733      |     0.9118     |     0.0615     |    0.9120     |    0.9680    |
| **Calibrated RF**                  |     0.9753      |     0.8937     |     0.0816     |    0.8940     |    0.9594    |
| **Calibrated Logistic Regression** |     0.8079      |     0.8153     |    -0.0074     |    0.8160     |    0.8834    |
| **Baseline: Constructor Tier**     |     0.7779      |     0.8098     |    -0.0320     |    0.8100     |    0.8128    |
| **Baseline: Driver Rolling Avg**   |     0.7689      |     0.7689     |    -0.0000     |    0.7700     |    0.8858    |
