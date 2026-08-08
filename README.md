# CS5100 Final Project: Student Phone Addiction & Productivity Prediction

**Authors:** Emma Bartnick, Elizabeth Fallat, and Even Laukli

## What this project does
This repository analyzes a dataset of daily digital habits (screen time, social media use, notifications, sleep, coffee intake, etc.) to explore their relationship with productivity and phone addiction level. This code cleans and validates the raw data, encodes categorical variables, fills missing values, addresses invalid/outlier values, and prepares train/test splits for modeling `productivity_score`.

## Repository contents
| File | Description |
|---|---|
| `scripts/productivity.ipynb` | Loads the raw CSV, checks for missing/invalid values and duplicate rows, encodes `addiction_level`, fills missing data, cleans invalid entries, checks correlations with `productivity_score`, and builds train/test splits |
| `data/student_phone_addiction_affects_on_productivity(in).csv` | Raw dataset (10,000 rows, 13 columns) |
| `data/cleaned_phone_addiction_vs_productivity.csv` | Cleaned dataset |
| `requirements.txt` | Python library versions needed to reproduce this work |
| `README.md` | This file |

## Dataset
**Digital Habits Dataset**: a synthetic/collected dataset of 10,000 individual daily records capturing screen time, digital habits, and derived wellbeing/performance metrics.

Source: [Student's Social Media Habits vs. Productivity](https://www.kaggle.com/datasets/racchie/students-social-media-habits-vs-productivity) (Kaggle)

- **10,000 rows**, one record per individual per day
- **13 columns**: `age`, `daily_screen_time`, `social_media_hours`, `study_hours`, `sleep_hours`, `notifications_per_day`, `focus_score`, `coffee_per_day`, `breaks_per_day`, `night_usage`, `distraction_score`, `addiction_level`, `productivity_score`
- **Target variable**: `productivity_score` (continuous, regression)

Missing values were present across nearly every column (~2-10% per column, with `distraction_score` missing ~10%). Several columns also contained physically invalid values (e.g. negative `notifications_per_day`, negative `distraction_score`) that required cleaning rather than simple imputation.

Following preprocessing there is a baseline OLS regression and a random forest regressor used to predict productivity_score. Additionally, there is a shallow (max depth = 5) and a deeper random forest classifier that are used to predict a self-created productivity_level (Low/Medium/High) target binned from productivity_score. The regression model is evaluated with MAE, RMSE, and R² and the classification model is evaluated with accuracy, precision, recall, F1, and a confusion matrix.

## How to run

1. Clone this repository.
2. Confirm `data/student_phone_addiction_affects_on_productivity(in).csv` is present (or update the path in the notebook if you've placed it elsewhere).
3. Create a virtual environment and install dependencies:
   ```bash
   python -m venv venv
   source venv/bin/activate    # Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```
4. Open `scripts/productivity.ipynb` in Jupyter or VS Code. It:
   - Loads `student_phone_addiction_affects_on_productivity(in).csv` and checks for missing/NaN values
   - Encodes `addiction_level` (Low = 0, Medium = 1, High = 2) after filling missing categorical entries with the mode
   - Fills missing numeric values using the median (chosen after checking skew per column, since several features like `focus_score` and `notifications_per_day` are notably skewed)
   - Checks for and drops duplicate rows
   - Checks initial correlations between each feature and `productivity_score`
   - Clips physically invalid negative values in `notifications_per_day` and `distraction_score` to 0
   - Caps extreme outliers in `notifications_per_day` at the 99.9th percentile
   - Leaves `productivity_score` (the target) unclipped, since its distribution naturally extends below 0 and above 100
   - Splits the data into train/test sets before scaling to avoid data leakage
   - Scales features using `StandardScaler`, fit on the training set only
5. Run all cells from top to bottom (**Restart Kernel + Run All**). The final cells output the cleaned, encoded, and scaled `X_train`/`X_test` data and corresponding target values, ready for model training.