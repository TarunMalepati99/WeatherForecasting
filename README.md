# 🌍 Global Weather Trend Forecasting

> Advanced Data Science Assessment | PM Accelerator Program

A comprehensive end-to-end analysis of the [Global Weather Repository](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository/code) dataset — covering 137,000+ weather records across 211 countries from May 2024 to April 2026.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Analysis Pipeline](#analysis-pipeline)
- [Results](#results)
- [Requirements](#requirements)
- [Usage](#usage)
- [Key Findings](#key-findings)
- [About PM Accelerator](#about-pm-accelerator)

---

## Overview

This notebook delivers a full-stack weather analytics pipeline — from raw data ingestion to ensemble forecasting and spatial visualization. It is designed to run end-to-end with either the real Kaggle dataset or an auto-generated synthetic dataset with the same schema.

---

## Dataset

**Source:** [Global Weather Repository – Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository/code)

| Attribute | Value |
|-----------|-------|
| Records | 137,413 |
| Countries | 211 |
| Features | 41 columns |
| Date Range | May 2024 – April 2026 |

**Key features include:** temperature, humidity, wind speed/direction, pressure, precipitation, UV index, visibility, moon phase, and 8 air quality metrics (PM2.5, PM10, CO, O₃, NO₂, SO₂, EPA index, DEFRA index).

To use the real dataset, download `GlobalWeatherRepository.csv` from Kaggle and place it in the project root. The notebook will detect it automatically.

---

## Project Structure

```
├── GlobalWeatherTrendForecasting.ipynb   # Main notebook
├── GlobalWeatherRepository.csv           # Dataset (download from Kaggle)
├── README.md
└── outputs/
    ├── eda_temp_precip.png
    ├── eda_correlation.png
    ├── eda_timeseries.png
    ├── eda_decomposition.png
    ├── anomaly_detection.png
    ├── forecast_all_models.png
    ├── model_comparison.png
    ├── feature_importance.png
    ├── climate_analysis.png
    ├── geo_temp_map.png
    └── ...
```

---

## Analysis Pipeline

### 0. Environment Setup
All dependencies loaded and configured — supports optional XGBoost and LightGBM.

### 1. Data Loading
Auto-detects the Kaggle CSV or falls back to a synthetic dataset for immediate execution.

### 2. Data Cleaning & Preprocessing
- Datetime parsing and time-feature engineering (month, season, day-of-year, quarter)
- Median imputation for missing values
- IQR-based outlier clipping on key weather variables

### 3. Exploratory Data Analysis (EDA)
- Temperature and precipitation distributions
- Seasonal boxplots and monthly mean charts
- Feature correlation heatmap
- Daily time-series with 30-day rolling mean
- Seasonal decomposition (trend + seasonal + residual)

### 4. Anomaly Detection
Three complementary methods applied to temperature data:

| Method | Anomalies Found |
|--------|----------------|
| Z-Score (\|z\| > 3) | 931 (0.68%) |
| IQR (1.5× fence) | 2,382 (1.73%) |
| Local Outlier Factor | 100 (2% contamination) |

### 5. Forecasting — Multiple Models
Lag features (14 lags + rolling statistics + cyclical encoding) are used to train and compare:

| Model | RMSE | MAE | R² |
|-------|------|-----|-----|
| **Ridge Regression** | **1.048** | **0.386** | **0.714** |
| Linear Regression | 1.049 | 0.389 | 0.713 |
| Ensemble (avg ML) | 1.457 | 0.901 | 0.447 |
| Random Forest | 1.786 | 1.166 | 0.168 |
| Gradient Boosting | 1.797 | 1.234 | 0.158 |
| XGBoost | 1.847 | 1.325 | 0.110 |
| LightGBM | 1.866 | 1.292 | 0.091 |
| SARIMA(1,1,1)(1,1,1,7) | 4.510 | 3.319 | -4.49 |

### 6. Feature Importance
Random Forest native importance + permutation importance. Top predictors:

1. `rolling_mean_7` — 0.492
2. `lag_1` — 0.330
3. `lag_3` — 0.078
4. `lag_2` — 0.073
5. `lag_4` — 0.016

### 7. Climate Analysis
- Annual temperature trends with linear regression overlay
- Monthly climatology boxplots
- Country-level mean temperature and variability

### 8. Air Quality Analysis
Breakdown of PM2.5, PM10, CO, O₃, NO₂, SO₂ by country and correlation with weather parameters.

### 9. Spatial & Geographical Analysis
- Global scatter map of country mean temperatures
- Continent-level weather comparisons (temperature, humidity, precipitation, wind)

---

## Results

**Best model:** Ridge Regression — RMSE 1.05°C, R² 0.714

**Global climate stats:**
- Mean temperature: 21.3°C
- Range: −29.8°C to 79.3°C

**Key insight:** Short-lag features (7-day rolling mean, 1-day lag) dominate predictive power, confirming strong temporal autocorrelation in global temperature data.

---

## Requirements

```bash
pip install numpy pandas matplotlib seaborn scipy statsmodels scikit-learn xgboost lightgbm
```

| Package | Purpose |
|---------|---------|
| `pandas` / `numpy` | Data wrangling |
| `matplotlib` / `seaborn` | Visualization |
| `scipy` / `statsmodels` | Statistics & SARIMA |
| `scikit-learn` | ML models, anomaly detection |
| `xgboost` / `lightgbm` | Gradient boosting (optional) |

---

## Usage

1. Clone the repo and install dependencies
2. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository/code) and place `GlobalWeatherRepository.csv` in the root directory
3. Open `GlobalWeatherTrendForecasting.ipynb` in Jupyter or Google Colab
4. Run all cells — outputs are saved as PNG files

```bash
git clone https://github.com/your-username/global-weather-forecasting
cd global-weather-forecasting
pip install -r requirements.txt
jupyter notebook GlobalWeatherTrendForecasting.ipynb
```

---

## Key Findings

1. **Data Quality** — Minimal missing values; IQR clipping preserved genuine climate variation while removing extreme outliers.
2. **Temporal Patterns** — Clear seasonal cycles confirmed by decomposition. Lag features are the strongest ML predictors.
3. **Forecast Accuracy** — Linear models outperformed tree-based methods on this lag-feature setup. SARIMA underperformed due to limited training length.
4. **Climate Insights** — Tropical regions are warmer and more stable; northern latitudes show stronger annual variability. A slight warming trend is visible year-over-year.
5. **Air Quality** — PM2.5 concentrations are highest in South/East Asian urban centers. Wind speed negatively correlates with AQI (dispersion effect).
6. **Spatial Patterns** — Latitude is the dominant driver of mean temperature; continental interiors show wider temperature swings than coastal locations.

---

## About PM Accelerator

> PM Accelerator empowers aspiring and experienced product managers with world-class education, mentorship, and community — democratizing access to top-tier PM training through hands-on learning, real-world projects, and industry connections.

🔗 [Learn more about PM Accelerator](https://www.pmaccelerator.io)

---

*Built with ❤️ as part of the PM Accelerator Data Science Assessment*
