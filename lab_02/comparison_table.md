# Lab 1 vs Lab 2 — Comparison Table

## Tomás Solano

| Model / Baseline         | Accuracy | Precision | Recall | F1   | ROC-AUC |
| ------------------------ | -------- | --------- | ------ | ---- | ------- |
| Majority class (Lab 1)   | 0.50     | 0.00      | 0.00   | 0.00 | 0.50    |
| Domain heuristic (Lab 1) | 0.71     | 0.92      | 0.53   | 0.68 | 0.68    |
| Lab 2 model (LogReg)     | 0.78     | 0.79      | 0.74   | 0.75 | 0.78    |

## Primary metric: Recall (justified in Lab 1)

## Interpretation

> For the most part the logistic model beat the baselines, except for precision, which make sense, one of the reasons why precision was not selected as primary metric was because.
> When It came down to getting balanced data between negatives and positives, however, recall tells us the actual top-10s, false positives are some of the most costly parts of a model, since we would miss a top 10 finisher, knowing that recall was one of the biggest improvement over all going from **0.53** in our heuristic baseline all the way to **0.74** -> **0.21** difference.

> Majority class was easier to beat since It could only predict for the non-top 10 (0.50 accuracy), the hardest one was Domain Heruistic by far, and even then precision took a hit, however, in lab 1, we wanted to beat the score of 0.71, which was accomplished with our new model.

> This really shows the importance of feature engineering, in this case we could use some of our own knowledge about F1, but when that isn't possible (unstructured data, inevaluable data e.g. company's workers data or similar) a system that can helps us get the best features will be one of if not the biggest factors when It comes down to the "usability" of a model.
