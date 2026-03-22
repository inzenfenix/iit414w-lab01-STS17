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

---

## Heuristics:

> Context: Wasn't sure were to start when thinking about a baseline and what heuristic could be a good choice for F1 races.

> Prompt: 
```
 What heuristics are a good starting point to understanding the data given from FastF1 and jolpica API?
 Give it to me as a list
```

> Relevant Output: 
``` md
1. The "Constructor Power" Heuristic

The Rule: "If a driver belongs to a Top 3 team, they will finish in the Top 10 regardless of their grid position."

    Domain Reasoning: In the hybrid era, cars from Red Bull, McLaren, and Ferrari rarely finish outside the points unless they DNF. This is great for catching drivers who had a bad qualifying session but "carved through the field" during the race.

    Implementation: predict = 1 if team in ['Red Bull', 'McLaren', 'Ferrari'] else 0.

2. The "Teammate Alpha" Heuristic

The Rule: "If a driver qualified ahead of their teammate, they are more likely to finish in the Top 10."

    Domain Reasoning: F1 is unique because your primary benchmark is your teammate in the same car. If a driver wins the intra-team qualifying battle, it usually indicates they have the better setup or "form" for that specific weekend.

    Implementation: Compare session.results for both drivers of the same TeamName.

3. The "Historical Track King" Heuristic

The Rule: "If a driver finished in the Top 10 at this specific track last year, they will do it again."

    Domain Reasoning: Certain drivers excel at specific types of tracks (e.g., "Street Circuit Specialists" like Perez or Leclerc). This heuristic uses temporal logic—using last year's data to predict this year's race.

    Implementation: This is where you would use the Jolpica API to GET the 2024 results for the same circuit_id and map them to your 2025/2026 validation set.

4. The "Points Momentum" Heuristic

The Rule: "If the driver is currently in the Top 10 of the World Drivers' Championship (WDC) standings, predict a Top 10 finish."

    Domain Reasoning: Consistency is the best predictor of future performance. If they have consistently scored points all season, they are statistically more likely to score in the next race than a driver at the bottom of the standings.

    Implementation: Use fastf1.get_event_schedule() to find the previous race, then pull the standings up to that point.
```

> Validation: Used one of them that interested me thinking about the fact that we are working with the constructor team (first one)

> Adaptations: Didn't change much since it run as intended.

> Final Decision: Used

---

## Requirements.txt 2.0:

> Context: Last time got a deduciton on score because of the usage of <= so i wanted to know which versions would be the best of each library for my current python version.

> Prompt: 
```
 You said

jupyterlab>=4.0

ipykernel>=6.25

numpy>=1.23

pandas>=1.5

scikit-learn>=1.2

matplotlib>=3.6

seaborn>=0.12

fastf1>=3.0, can you upgrade this to their python 3.13.4 versions? and use "==" 
```

> Relevant Output: 
``` bash
jupyterlab==4.3.5
ipykernel==6.29.5
numpy==2.2.3
pandas==2.2.3
scikit-learn==1.6.1
matplotlib==3.10.0
seaborn==0.13.2
fastf1==3.8.1
```

> Validation: Tried to run it with pip on a clean install of a new virtual environment and it worked as intended.

> Adaptations: Didn't change much since it run as intended.

> Final Decision: Used


---

## Starting point


> Context: Wanted to get a starting point to understand the context of the task at hand

> Prompt: 
```
 Baseline task
```

> Relevant Output: 
``` bash
To implement a baseline for F1 prediction using FastF1 and the Jolpica API, you need to focus on "Pre-Race" data. The most common heuristic for a Top 10 finish is Grid Position (Qualifying result), as starting near the front is the strongest predictor of finishing there.
```

``` python
import fastf1
import requests
import pandas as pd

# 1. Get 2024 Season Schedule via Jolpica (Example)
response = requests.get("https://api.jolpica.com/ergast/f1/2024.json")
data = response.json()
races = data['MRData']['RaceTable']['Races']

# 2. Heuristic Baseline Function
def predict_top_10_heuristic(grid_position):
    """Rule: If you start in the top 10, you finish in the top 10."""
    return 1 if grid_position <= 10 else 0

# 3. Collect Validation Data (e.g., last 3 races of 2024)
validation_results = []
race_numbers = [22, 23, 24] # Las Vegas, Qatar, Abu Dhabi

for r in race_numbers:
    session = fastf1.get_session(2024, r, 'R')
    session.load(laps=False, telemetry=False)
    results = session.results[['FullName', 'GridPosition', 'ClassifiedPosition']]
    
    for _, row in results.iterrows():
        # Handle DNFs or unclassified if necessary
        try:
            actual_pos = int(row['ClassifiedPosition'])
            actual_top_10 = 1 if actual_pos <= 10 else 0
            predicted_top_10 = predict_top_10_heuristic(row['GridPosition'])
            
            validation_results.append({
                'actual': actual_top_10,
                'predicted': predicted_top_10
            })
        except ValueError:
            continue # Skip non-classified drivers

df_val = pd.DataFrame(validation_results)

# 4. Compute Accuracy (Requirement 4.2)
accuracy = (df_val['actual'] == df_val['predicted']).sum() / len(df_val)
print(f"Heuristic Baseline Accuracy: {accuracy:.2%}")
```

> Validation: Tested the code, but modified it to the question got at the prior point (EDA Requirements)

> Adaptations: 
``` python
# ── Cell 3: Domain Heruistic Baseline ───────────────────────────────────────────────
# Check the rule that If a driver belongs to a Top 3 team, they will finish in the Top 10 regardless of their grid position

import fastf1
import pandas as pd


def top_3_team_is_top_10_heuristic(driver):
    top_teams = ['Red Bull Racing', 'McLaren', 'Ferrari']
    return 1 if driver["TeamName"] in top_teams else 0

print("------------")
print("\nRule: If a driver belongs to a Top 3 team, they will finish in the Top 10 regardless of their grid position.\n")
print("------------")

validation_results = []
data_df = pd.DataFrame({ "TeamName": [], "Classified Position": []})
race_nums = [19, 20, 21, 22, 23, 24]

for race_num in race_nums:
    session = fastf1.get_session(2024, race_num, 'R')
    session.load(laps=False, telemetry=False)
    
    results = session.results[['FullName', 'ClassifiedPosition', 'TeamName']]
    
    
    for _, row in results.iterrows():
        # Handle DNFs or unclassified if necessary
        # Also get the data as 0s and 1s to calculate accuracy
        data_df.loc[-1] = [row["TeamName"], row["ClassifiedPosition"]]
        data_df.index = data_df.index + 1
        
        top_10_binary_converter = lambda x: 1 if x <= 10 else 0
        try:
            actual_pos = int(row['ClassifiedPosition'])
            actual_top_10 = top_10_binary_converter(actual_pos)
            predicted_top_10 = top_3_team_is_top_10_heuristic(row)

            
            validation_results.append({
                'actual': actual_top_10,
                'predicted': predicted_top_10

            })
        except (ValueError, TypeError):
            continue # Skip non-classified drivers

df_val = pd.DataFrame(validation_results)
```

> Final Decision: Partially Used
