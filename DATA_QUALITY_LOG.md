# Data Quality Log — Lab 1

## Issue 1: Classified Position — Multitype column
- **What:** This column for the most part contains numbers, but it uses "R" and other strings for those that weren't on a race or got disqualified.
- **Classification:** MNAR
- **Impact:** Not much since It's quite easy to zero-out
- **Decision:** Mutate, transform it to a 0, meaning it wasn't top 10
- **Justification:** It makes sense in the context of my baseline, they really weren't on the top 10 if they were disqualified.