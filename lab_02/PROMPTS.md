# Tomás Solano, InZenFenix, IIT414W, 22-03-2026

## 1. Starting point

> Context: Wanted to understand a bit more of how what data could be used for our model.

> Prompt:

```
What other features could i use from F1 Datasets (both fastf1 and jolpica api) to predict if someone gets top 10 on a race? (Preventing leakage, and how can i get at least 3 of this features onto a pandas dataframe) 
```

Relevant Output:

High-Value Feature Ideas

Beyond the obvious qualifying position, these three categories offer the best predictive signals:

    Average Pit Stop Duration (Team Efficiency): A slow stop can ruin a race. Using the average pit time of a driver’s team over the last 5 races acts as a "Team Strength" indicator.

    Sector-Specific Performance (Track Suitability): Some cars excel in high-speed sectors (Sector 1 in Spa) but struggle in technical ones. Comparing a driver’s fastest qualifying sector time against the session median tells you if the car "fits" the track.

    Rolling Finishing Position (Consistency): The "form" of a driver. A rolling average of the last 3–5 races captures momentum better than a full-season average.

> Final Decision: Partially used

> Validation: Got the starting idea to understand what I needed to get going

> Adaptations: No adaptation since I was just asking for data

> Final Decision: Used

--- 


## 2. Features:

> Context: Since my knowledge on F1 is fairly limited, I asked a bit of what features i could use for the lab

> Prompt: 
```
Feature A: Rolling Average Finish (Rolling Aggregate)

    Definition: The average finishing position of a driver over the previous 5 races.

    Justification: This captures "current form" and reliability. A driver on a hot streak is statistically more likely to maintain that momentum than their career average suggests.

    Leakage Guard: Use .shift(1) in Pandas so the "Current Race" results are never included in the average.

Feature B: Grid-to-Finish Reliability (Lag Feature)

    Definition: The difference between GridPosition and ClassifiedPosition from the previous race.

    Justification: This acts as a proxy for "Race Pace" vs. "Qualifying Pace." Some cars are "Saturday Cars" (fast in qualifying but drop back in the race); this helps the model adjust the starting grid bias.

    Leakage Guard: This must be lagged by at least one event.

Feature C: Constructor Tier (Categorical Encoding)

    Definition: Mapping teams into 3 tiers (Top, Mid, Bottom) based on their total points from the previous season.

    Justification: F1 is a developmental sport where car performance is grouped by budget and design philosophy; a "Top Tier" car (e.g., Red Bull/Mercedes) has a significantly higher floor for a Top 10 finish regardless of the driver.

    Leakage Guard: Use the previous year's standings, which are fixed before the current season begins.
```

> Validation: Used the features, and made sure that the implementation made sense

> Adaptations: Just asked for data, but did paraphrase it while explaining why I wanted this 3 features.

> Final Decision: Used

---

## 3. Implementation:

> Context: Since I already had a template for how I handled my data i gave that to Gemini so It creates code based on my coding system

> Prompt: 
```
this is how i got my data temporally on lab 1: **Data code from lab 1**
```

> Relevant Output: 
``` python
import fastf1
import pandas as pd
import numpy as np
from sklearn.linear_model import LogisticRegression

# Requirements
RANDOM_SEED = 414

# 1. Define Constructor Tiers based on 2023 Season Results (Pre-race knowledge)
# This serves as our Categorical/Ordinal Encoding feature
tier_map = {
    'Red Bull Racing': 3, 'Mercedes': 3, 'Ferrari': 3, 'McLaren': 3,
    'Aston Martin': 2, 'Alpine': 2, 'Williams': 2,
    'Haas F1 Team': 1, 'RB': 1, 'Kick Sauber': 1
}

# 2. Collect Data chronologically
all_race_data = []
race_nums = range(1, 24) # Expanded range to ensure we have "history" for rolling averages

for race_num in race_nums:
    try:
        session = fastf1.get_session(2024, race_num, 'R')
        session.load(laps=False, telemetry=False)
        
        # We need Grid for our model and Position for our 'Form' calculation
        res = session.results[['FullName', 'TeamName', 'GridPosition', 'ClassifiedPosition']]
        res['Round'] = race_num
        all_race_data.append(res)
    except:
        continue

full_df = pd.concat(all_race_data).reset_index(drop=True)

# 3. Feature Engineering (The 3 New Features)

# Helper to clean position strings (converts 'R', 'W', etc. to 20 for math purposes)
def clean_pos(x):
    return int(x) if str(x).isdigit() else 20

full_df['PosNumeric'] = full_df['ClassifiedPosition'].apply(clean_pos)

# FEATURE 1: Rolling Average (Rolling Aggregate)
# Average finish of the last 3 races. We SHIFT(1) to prevent leakage.
full_df['rolling_form'] = full_df.groupby('FullName')['PosNumeric'].transform(
    lambda x: x.shift(1).rolling(window=3, min_periods=1).mean()
)

# FEATURE 2: Grid Position (Direct Pre-race Feature)
# This is naturally pre-race, so no shift needed.
full_df['grid_num'] = full_df['GridPosition'].astype(int)

# FEATURE 3: Team Tier (Categorical/Ordinal Encoding)
full_df['team_tier'] = full_df['TeamName'].map(tier_map).fillna(1)

# Target Variable
full_df['IsTop10'] = full_df['PosNumeric'].apply(lambda x: 1 if x <= 10 else 0)

# 4. Final Cleanup & Temporal Split
# Drop rows where we don't have rolling history (the first race for each driver)
data_df = full_df.dropna(subset=['rolling_form']).sort_values('Round')

# Selecting our 3 Engineered Features + Target
features = ['grid_num', 'rolling_form', 'team_tier']
X = data_df[features]
y = data_df['IsTop10']

# Maintain your 67% Temporal Split
split_index = int(len(data_df) * 0.67)

X_train = X.iloc[:split_index]
X_test = X.iloc[split_index:]
y_train = y.iloc[:split_index]
y_test = y.iloc[split_index:]

# 5. Simple Lab 2 Model
model = LogisticRegression(random_state=RANDOM_SEED)
model.fit(X_train, y_train)

print(f"Features used: {features}")
print(f"Training samples: {len(X_train)}, Testing samples: {len(X_test)}")
```

> Validation: Run as intended, checked that there was no leakage and that it was using my temporal validation just as I did on lab 1 (0.67).

> Adaptations: Didn't change much since it run as intended.

> Final Decision: Used
