1. EDA Structure

Context: I was starting the Exploratory Data Analysis section for my dataset created using FastF1 race results and was unsure how to structure the EDA questions so they aligned with the lab rubric requirements.

Prompt:

I'm working with fastf1 race results and need to start the EDA section. how should i structure the EDA questions and what should be the first question?

Relevant Output:

EDA

Question 1 — Class balance
Question 2 — Grid position vs Top-10
Question 3 — Team performance
Question 4 — Temporal patterns
Question 5 — Correlation analysis

Validation: Checked the rubric requirements and confirmed that the suggested questions aligned with required sections such as class balance, temporal analysis, and correlation analysis.

Adaptations: Adapted the questions slightly to match the variables present in my dataset (GridPosition, TeamName, year, top_10).

Final Decision: Used

2. Grid Position explanation

Context: While analyzing the dataset I noticed the column GridPosition, but I wasn't fully sure how it related to the race results and whether it was an important feature.

Prompt:

what is grid position in formula 1 and why would it be important for predicting race outcomes?

Relevant Output:

Grid position represents the starting position of a driver on the race grid.
Drivers who start closer to the front typically have a higher chance of finishing in better positions due to track position advantages and fewer overtaking requirements.

Validation: Compared the explanation with official Formula 1 race rules and confirmed that grid position comes from qualifying sessions and strongly influences race outcomes.

Adaptations: Used the explanation to justify the EDA question analyzing GridPosition vs Top-10 finishes.

Final Decision: Used

3. Fixing seaborn boxplot error

Context: While creating a boxplot to visualize the relationship between GridPosition and Top-10 finishes, seaborn raised an error related to duplicate index labels.

Prompt:

i'm getting this error when trying to plot a seaborn boxplot: ValueError: cannot reindex on an axis with duplicate labels. how can i fix it?

Relevant Output:

df = df.reset_index(drop=True)

plt.figure(figsize=(8,5))

sns.boxplot(
    data=df,
    x="top_10",
    y="GridPosition"
)

Validation: Reset the dataframe index before plotting and verified that the visualization worked without errors.

Adaptations: Only applied the reset_index(drop=True) step since the rest of the plotting code was already correct.

Final Decision: Used

4. Data Quality Audit formatting

Context: My Data Quality Audit initially used multiple print() statements which produced messy output in the notebook. I wanted to present the dataset overview and missing values in a cleaner format.

Prompt:

how can i display dataset information like shape, missing values and column types in a cleaner way in a jupyter notebook using pandas dataframes instead of print statements?

Relevant Output:

missing = pd.DataFrame({
    "Column": df.columns,
    "MissingValues": df.isna().sum(),
    "MissingPercent": (df.isna().sum() / len(df) * 100).round(2)
})

display(missing.sort_values("MissingValues", ascending=False))

Validation: Implemented the suggested dataframe-based display and confirmed that the audit section became easier to read and interpret in the notebook.

Adaptations: Combined the output with additional tables for dataset overview, data types, and duplicate rows.

Final Decision: Partially Used