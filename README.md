# Population Growth vs. Air Pollution: Predicting PM2.5 AQI with Machine Learning

Code for my research paper on predictive analytics for air pollution levels, published and presented at **ICCSCI 2024** (International Conference on Computer Science and Computational Intelligence).

## Overview

Does a country's population size and growth help explain how polluted its cities are? This project combines a global air quality dataset with urban/rural population data, then trains and compares five regression models to predict **PM2.5 AQI** from population features.

## Data

| Dataset | Contents |
|---|---|
| `global air pollution dataset.csv` | City-level AQI values (overall, CO, Ozone, NO2, PM2.5) and AQI categories |
| `urbanandruralpopulation new.csv` | Country-level urban and rural population by year |

The datasets are not included in this repo. Place both CSV files in the project root before running the notebook.

## Method

1. **Merge** the two datasets on country.
2. **Feature engineering**: compute urban and rural population growth rates with `pct_change()`, then drop rows with missing values.
3. **Features**: urban population, rural population, urban growth rate, rural growth rate.
4. **Target**: PM2.5 AQI value.
5. **Split** 80/20 into train and test sets (`random_state=42`), with standard scaling for the scale-sensitive MLP model.
6. **Train and compare** five models: Linear Regression, Ridge Regression, Random Forest, XGBoost, and MLP Regressor.

## Results (test set)

| Model | RMSE | MAE | R² |
|---|---|---|---|
| Linear Regression | 48.87 | 31.84 | 0.321 |
| Ridge Regression | 48.87 | 31.83 | 0.321 |
| **Random Forest** | **43.33** | **26.24** | **0.466** |
| XGBoost | 43.45 | 26.44 | 0.463 |
| MLP Regressor | 45.80 | 29.20 | 0.409 |

**Random Forest performed best**, with XGBoost close behind. The tree-based models clearly beat the linear models, suggesting the relationship between population and PM2.5 is non-linear.

## Limitations

- **Correlation, not causation.** Population features explain under half of the variance in PM2.5 (R² ≈ 0.47). Pollution also depends on industry, energy mix, traffic, weather and regulation, none of which are in this data.
- **Granularity mismatch.** Pollution is measured per city, while population is per country, so all cities in a country share the same population features.
- **Possible leakage.** The merge is on country only, so each city's pollution reading is repeated across population years. A random train/test split can place near-duplicate rows in both sets. Splitting by city or by year would give a more conservative estimate.

## Tech Stack

Python · pandas · NumPy · scikit-learn · XGBoost · Matplotlib · Seaborn

## How to Run

```bash
pip install pandas numpy scikit-learn xgboost matplotlib seaborn jupyter
jupyter notebook Research.ipynb
```

## Author

**Darel Yeshe**, Data Science graduate, Bina Nusantara University
