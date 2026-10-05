# Interpretable-Wind-Power-Forecasting

This repository contains the implementation for my Summer 2026 retake project for the module **Interdisciplinary Elective: AI, Power & Responsibility**.

The project investigates short-horizon wind power forecasting using operational wind turbine measurements. A Random Forest regression model is compared with a simple persistence baseline for predicting wind power **30 minutes ahead**.

## Project Objective

The objective is to answer the following prediction question:

> Can current operational and weather measurements be used to predict wind turbine power output 30 minutes ahead, and how does a Random Forest model compare with a persistence baseline?

The project also examines Random Forest feature importance to provide an interpretable view of which input variables contribute most strongly to the model.

## Dataset

The project uses data from the **Kelmarsh Wind Farm Data** dataset available through Zenodo.

Dataset source:

https://zenodo.org/records/16807551

The experiment uses 2020 SCADA data with a sampling interval of 10 minutes.

The main variables used are:

- Power (kW)
- Wind speed (m/s)
- Wind direction (°)
- Time-derived cyclical features

The target variable is wind power **30 minutes into the future**.

The raw dataset is not included in this repository. It can be downloaded from the Zenodo source above.

## Forecasting Setup

Because the measurements are time ordered, the data are split chronologically rather than randomly:

- Training: first 60%
- Validation: next 20%
- Test: final 20%

After preprocessing, the modelling dataset contains **52,170 usable observations**:

- Training: 31,302 observations
- Validation: 10,434 observations
- Test: 10,434 observations

The held-out test set is used only for final evaluation.

## Features

The final model uses seven input features:

- Current power
- Wind speed
- Wind direction
- Hour sine
- Hour cosine
- Day-of-year sine
- Day-of-year cosine

The cyclical features represent daily and seasonal time patterns.

## Models

### Persistence Baseline

The persistence baseline assumes that power 30 minutes into the future will be equal to the current power measurement.

### Random Forest

Three Random Forest configurations are compared using the validation set.

The selected configuration is:

- Number of trees: 200
- Maximum depth: None
- Minimum samples per leaf: 5
- Random state: 42

The selected model is retrained using the combined training and validation data before evaluation on the held-out test set.

## Final Test Results

| Model | MAE (kW) | RMSE (kW) | R² |
|---|---:|---:|---:|
| Persistence | 153.33 | 240.81 | 0.8757 |
| Random Forest | 165.98 | 239.33 | 0.8772 |

The persistence baseline achieves the lower MAE, while the Random Forest achieves a slightly lower RMSE and slightly higher R². Therefore, the Random Forest does not consistently outperform persistence across all evaluation metrics.

## Feature Importance

Random Forest feature importance shows that **current power is the dominant predictor** for this 30-minute forecasting task.

The other features, including wind speed, wind direction, and cyclical time features, have substantially smaller model-specific importance values.

This importance should not be interpreted as a causal measure or as evidence that weather variables are physically unimportant for wind generation.

## Repository Structure

```text
Interpretable-Wind-Power-Forecasting/
│
├── README.md
├── Interpretable_Wind_Power_Forecasting.ipynb
├── requirements.txt
│
└── results/
    ├── final_test_results.csv
    ├── validation_results.csv
    ├── final_feature_importance.csv
    ├── final_forecast_comparison.png
    └── final_feature_importance.png
