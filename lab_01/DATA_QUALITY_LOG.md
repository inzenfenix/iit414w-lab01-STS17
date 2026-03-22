# Data Quality Log — Lab 1

## Issue 1: Classified Position — Multitype column
- **What:** This column for the most part contains numbers, but it uses "R" and other strings for those that weren't on a race or got disqualified.
- **Classification:** MNAR
- **Impact:** Not much since It's quite easy to zero-out
- **Decision:** Mutate, transform it to a 0, meaning it wasn't top 10
- **Justification:** It makes sense in the context of my baseline, they really weren't on the top 10 if they were disqualified.

## Issue 2: Categories — Model Making
- **What:** For the logistic regression model, the team name wasn't readily insertable, had to convert it using get_dummies().
- **Classification:** Type error
- **Impact:** Not much since it can be transformed
- **Decision:** Mutate, transform it using One-Hot Encoding
- **Justification:** It's a good way to get a non-numerical feature to fit into a model.

## Issue 3: Q1 — EDA
- **What:** NAT on qualifying sessions
- **Classification:** MNAR
- **Impact:** Not much since it's not used
- **Decision:** Leave it behind, we only need the information after qualifying
- **Justification:** We need data from after the qualifying sessions

## Issue 4: Q2 — EDA
- **What:** NAT on qualifying sessions
- **Classification:** MNAR
- **Impact:** Not much since it's not used
- **Decision:** Leave it behind, we only need the information after qualifying
- **Justification:** We need data from after the qualifying sessions

## Issue 5: Q3 — EDA
- **What:** NAT on qualifying sessions
- **Classification:** MNAR
- **Impact:** Not much since it's not used
- **Decision:** Leave it behind, we only need the information after qualifying
- **Justification:** We need data from after the qualifying sessions