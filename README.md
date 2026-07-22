# CS5100 Final Project: Student Phone Addiction & Productivity Prediction

## What this project does
This repository analyzes a dataset of daily digital habits (screen time, social media use, notifications, sleep, coffee intake, etc.) to explore their relationship with productivity and phone addiction level. This code cleans and validates the raw data, encodes categorical variables, fills missing values, addresses invalid/outlier values, and prepares train/test splits for modeling `productivity_score`.

## Repository contents
| File | Description |
|---|---|
| `productivity.ipynb` | Loads the raw CSV, checks for missing/invalid values, encodes `addiction_level`, fills missing data, cleans invalid entries, and builds train/test splits |
| `student_phone_addiction_affects_on_productivity(in).csv` | Raw dataset (10,000 rows, 13 columns) |
| `cleaned_phone_addiction_vs_productivity.csv` | Cleaned dataset |
| `requirements.txt` | Python library versions needed to reproduce this work |
| `README.md` | This file |

## Dataset
**Digital Habits Dataset**: a synthetic/collected dataset of 10,000 individual daily records capturing screen time, digital habits, and derived wellbeing/performance metrics.

- **10,000 rows**, one record per individual per day
- **13 columns**: `age`, `daily_screen_time`, `social_media_hours`, `study_hours`, `sleep_hours`, `notifications_per_day`, `focus_score`, `coffee_per_day`, `breaks_per_day`, `night_usage`, `distraction_score`, `addiction_level`, `productivity_score`
- **Target variable(s)**: `productivity_score` (continuous, regression) and/or `addiction_level` (categorical: Low / Medium / High)

Missing values were present across nearly every column (~2-10% per column, with `distraction_score` missing ~10%). Several columns also contained physically invalid values (e.g. negative `notifications_per_day`, negative `distraction_score`) that required cleaning rather than simple imputation.

## How to run

1. Clone this repository.
2. Place `student_phone_addiction_affects_on_productivity(in).csv` in the repo root (or update the path in the notebook).
3. Create a virtual environment and install dependencies:
   ```bash
   python -m venv venv
   source venv/bin/activate    # Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```
4. Open `preprocessing.ipynb` in Jupyter or VS Code. It:
   - Loads `student_phone_addiction_affects_on_productivity(in).csv` and checks for missing/NaN values
   - Encodes `addiction_level` (Low = 0, Medium = 1, High = 2) after filling missing categorical entries with the mode
   - Fills missing numeric values using the median (chosen after checking skew per column, since several features like `focus_score` and `notifications_per_day` are notably skewed)
   - Clips physically invalid negative values in `notifications_per_day` and `distraction_score` to 0
   - Caps extreme outliers in `notifications_per_day` 
   - Leaves `productivity_score` (the target) unclipped, since its distribution naturally extends below 0 and above 100
   - Splits the data into train/test sets before scaling to avoid data leakage
   - Scales features using `StandardScaler`, fit on the training set only
5. Run all cells from top to bottom (**Restart Kernel + Run All**). The final cells output the cleaned, encoded, and scaled `X_train`/`X_test` data and corresponding target values, ready for model training.