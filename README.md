# Global Weather Data Analysis

Data science assessment project for analyzing the Global Weather Repository dataset. The work is organized as a notebook pipeline that moves from raw weather observations through cleaning, exploratory analysis, feature engineering, and forecasting.

Dataset: [Global Weather Repository - Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository/code)

## Repository Structure

```text
global-weather-data-analysis/
|-- README.md
|-- PM Accelator - Weather analysis assessement report.pdf
|-- code/
|   |-- Data cleaning & preprocessing.ipynb
|   |-- Exploratory data analysis (EDA).ipynb
|   |-- Feature engineering.ipynb
|   `-- Forecasting.ipynb
|-- data/
|   |-- GlobalWeatherRepository.csv
|   |-- GlobalWeatherRepository_Cleaned.csv
|   |-- GlobalWeatherRepository_Cleaned_Scaled.csv
|   `-- GlobalWeatherRepository_Engineered.csv
`-- plots/
```

## Workflow

Run the notebooks in this order:

1. `code/Data cleaning & preprocessing.ipynb`
   - Loads `data/GlobalWeatherRepository.csv`
   - Checks missing values, duplicates, invalid values, and outliers
   - Exports cleaned and scaled datasets

2. `code/Exploratory data analysis (EDA).ipynb`
   - Explores global temperature, humidity, precipitation, wind, UV index, weather conditions, and air quality
   - Generates visual outputs in `plots/`

3. `code/Feature engineering.ipynb`
   - Builds engineered weather, time, region, and air-quality features
   - Exports `data/GlobalWeatherRepository_Engineered.csv`

4. `code/Forecasting.ipynb`
   - Trains and evaluates forecasting and regression models
   - Compares baseline, tree-based, ensemble, and time-series approaches

## Data Files

| File | Description |
| --- | --- |
| `data/GlobalWeatherRepository.csv` | Raw Kaggle dataset |
| `data/GlobalWeatherRepository_Cleaned.csv` | Cleaned dataset after preprocessing |
| `data/GlobalWeatherRepository_Cleaned_Scaled.csv` | Cleaned dataset with scaled numeric features |
| `data/GlobalWeatherRepository_Engineered.csv` | Feature-engineered dataset used for modeling |

## Main Outputs

- Cleaned and transformed weather datasets in `data/`
- Exploratory charts and generated figures in `plots/`
- Forecasting/modeling notebook in `code/Forecasting.ipynb`
- Final assessment report: `PM Accelator - Weather analysis assessement report.pdf`

## Requirements

Python 3.11+ is recommended.

Install the main Python packages:

```bash
pip install pandas numpy matplotlib seaborn plotly scikit-learn xgboost statsmodels prophet
```

If you use Jupyter locally:

```bash
pip install notebook ipykernel
```

## How to Run

1. Open the project folder.
2. Confirm the raw dataset exists at `data/GlobalWeatherRepository.csv`.
3. Start Jupyter:

```bash
jupyter notebook
```

4. Run the notebooks from the `code/` folder in the workflow order above.

Some notebook cells reference Google Colab paths. If running locally, update those paths to the local `data/` directory before executing the cells.

---

## What We Did
 
### 1. Data Cleaning & Preprocessing
- No missing values or duplicates found
- Removed physically impossible sensor readings (temperature > 60°C, pressure > 1084 mb)
- Capped wind speed, precipitation, and air quality outliers at 99th percentile
- Applied **RobustScaler** for weather features and **MinMaxScaler** for bounded features
### 2. Exploratory Data Analysis
- Global temperature distribution and country-level boxplots
- Top 15 most polluted countries by PM2.5
- Humidity vs precipitation relationship
- Weather condition breakdown across 48 → 9 grouped categories
### 3. Feature Engineering
- Created 13 new features: season, heat index, wind chill, is_rainy, time_of_day, is_daytime, cyclical time encodings
- Compressed 6 air quality columns into 3 PCA components (82.5% variance retained)
- Dropped 9 unit-duplicate columns and 3 data-leaking features
- Binned UV index into WHO ordinal tiers
### 4. Model Building
| Model | MAE (°C) | RMSE (°C) | R² |
|-------|----------|-----------|-----|
| Linear Regression | 6.37 | 8.18 | 0.45 |
| Random Forest | 2.86 | 4.15 | 0.86 |
| Weighted Ensemble | 2.44 | 3.59 | 0.89 |
| **XGBoost (tuned)** | **2.36** | **3.45** | **0.90** |
| Prophet (Cairo) | 1.75 | 2.11 | -0.09* |
 
> *Negative R² explained by anomalous climate event — April 2026 was 5°C colder than April 2025 in Cairo.
 
### 5. Advanced Analyses
- **Ensemble Modeling:** Simple average, weighted (XGB 60% / RF 40%), and stacking ensembles
- **Climate Analysis:** Monthly temperature trends, year-over-year comparison (2025 was 2.56°C cooler than 2024)
- **Geographical Patterns:** Temperature, humidity, and seasonal variation across 186 countries and 5 regions

---
 
## Key Findings
 
- **XGBoost (tuned)** is the best model for global temperature prediction (R²=0.90)
- **Latitude** is the strongest predictor of temperature (38.2% feature importance)
- **Middle East** has the largest seasonal temperature swing (19°C winter to summer)
- **Southeast Asia** is the most climatically stable region globally
- **China, India, Saudi Arabia** are the most polluted countries by PM2.5
- **Data leakage** was discovered and fixed — heat index and wind chill were derived from the target variable
