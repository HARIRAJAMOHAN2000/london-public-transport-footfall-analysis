# **London Public Transport Footfall Analysis**

# Project Overview
This MSc Data Science project analyses daily public-transport footfall at two London stations between 1 January 2024 and 27 December 2025:

- **King’s Cross St Pancras**: a high-volume central interchange with mixed commuter, leisure and interchange demand.
- **Roding Valley**: a low-volume suburban station with a clearer commuter-led weekly pattern.

The project compares station demand patterns, detects unusual observations, and evaluates statistical and machine-learning forecasting models.

# Research Question
How do daily entry and exit footfall patterns in public transport stations change over time in 2024–2025?

# Dataset
Source: Transport for London Network Demand Data.

The dataset contains daily station-level aggregated tap-count records. The project uses:

| Variable | Description |
|---|---|
| `TravelDate` | Date of the station observation |
| `DayOfWeek` | Recorded day name |
| `Station` | Station name |
| `EntryTapCount` | Daily entry tap count |
| `ExitTapCount` | Daily exit tap count |
| `Footfall` | Derived total: `EntryTapCount + ExitTapCount` |

The original dataset contains 311,447 records across 437 stations. Only King’s Cross St Pancras and Roding Valley are used for the final comparative analysis.

> **Data note:** The dataset is aggregated at station-date level. It does not contain personal identifiers, passenger names, payment-card identifiers, addresses, demographics, or individual journey histories.

# Data Availability and License
The source dataset belongs to Transport for London and remains subject to TfL Transport Data Service terms and conditions.

The MIT licence in this repository applies only to the project code, notebook structure, and documentation. It does not transfer ownership of the TfL dataset or grant unrestricted rights to redistribute it.

Users should obtain, use, and share the source data according to TfL’s applicable terms.


# Analytical Workflow

The notebook follows this workflow: 

1. Load the station footfall dataset.
2. Clean column names, dates, station names, and tap-count values.
3. Create the target variable:
      `Footfall = EntryTapCount + ExitTapCount`
4. Filter the data to King’s Cross St Pancras and Roding Valley.
5. Aggregate duplicate station-date records.
6. Reindex each station to a complete daily time series.
7. Impute missing observations using weekday-median imputation, time interpolation, and forward/
backward completion.
8. Create calendar features including weekday, weekend flag, month, day, and ISO week number.
9. Perform exploratory data analysis, anomaly detection, forecasting, and model evaluation

# Exploratory Data Analysis

The analysis includes:
1. Daily footfall trend plots
2. Seven-day rolling averages
3. Monthly average daily footfall plots
4. Weekday seasonality indices
5. Footfall distribution plots
6. Mean and median reference lines
7. Robust z-score anomaly visualisations

# Forecasting Models
The following models are compared using a chronological final test period of 90 days:

| Model | Purpose |
|---|---|
| Seasonal Naive | Weekly baseline forecast using the previous seasonal cycle |
| Holt-Winters | Exponential smoothing with level, trend, and seasonality |
| SARIMA | Statistical seasonal autoregressive model selected using AIC |
| SARIMAX + Calendar | SARIMA extended with calendar-based predictors |
| XGBoost | Machine-learning model using lag, rolling, and calendar features |

## XGBoost Feature Engineering
The XGBoost model uses:
- Lag features: 1, 7, 14, 21, and 28 days
- Rolling-window features: 7, 14, and 28 days
-Rolling mean, standard deviation, minimum, and maximum
- Calendar variables: weekday, weekend indicator, month, day, and ISO week number

## Hyperparameter Tuning
XGBoost tuning uses `TimeSeriesSplit(n_splits=3)` to preserve chronological order and avoid future data leakage.

The following search grid contains `216 candidate combinations`:

| Hyperparameter | Values Tested |
|---|---|
| `n_estimators` | 300, 500, 800 |
| `learning_rate` | 0.03, 0.05, 0.10 |
| `max_depth` | 3, 4, 6 |
| `subsample` | 0.80, 0.90 |
| `colsample_bytree` | 0.80, 0.90 |
| `reg_lambda` | 1.00, 2.00 |

Mean validation RMSE across the time-series folds is used to select the final configuration.

## Final XGBoost Settings

| Station | `n_estimators` | `learning_rate` | `max_depth` | `subsample` | `colsample_bytree` |
|---|---:|---:|---:|---:|---:|
| King’s Cross St Pancras | 300 | 0.03 | 3 | 0.80 | 0.80 |
| Roding Valley | 300 | 0.03 | 3 | 0.80 | 0.90 |

## Evaluation Metrics
Forecasts are evaluated using:

| Metric | Meaning |
|---|---|
| MAE | Average absolute forecast error |
| RMSE | Error measure that gives more weight to large mistakes |
| MAPE | Average percentage error |
| sMAPE | Balanced percentage-based error measure |

Lower values indicate better performance


## Key Findings

| Station / Result | Main Finding |
|---|---|
| King’s Cross St Pancras | High-volume, volatile, and disruption-sensitive demand; 12 robust z-score anomalies were detected. |
| Roding Valley | Lower-volume and more regular commuter-led demand; no anomalies exceeded the selected threshold. |
| King’s Cross forecast result | XGBoost achieved the best MAE, MAPE, and sMAPE, while Holt-Winters achieved the lowest RMSE. |
| Roding Valley forecast result | XGBoost achieved the best MAE, RMSE, MAPE, and sMAPE. |


## Best Forecast Results
| Station | Best Model | Evidence |
|---|---|---|
| King’s Cross St Pancras | XGBoost overall | Lowest MAE: 13,425.71; MAPE: 7.65%; sMAPE: 7.52% |
| King’s Cross St Pancras | Holt-Winters for RMSE | Lowest RMSE: 30,359.16 |
| Roding Valley | XGBoost | Lowest MAE: 46.91; RMSE: 69.30; MAPE: 13.32%; sMAPE: 12.21% |

## Reproducing the Analysis

# Required Python Libraries
`pip install numpy pandas matplotlib seaborn statsmodels scikit-learn xgboost
jupyter`

# Run Locally
1. Clone or download this repository.
2. Open the project folder in Jupyter Notebook or Visual Studio Code.
3. Ensure that the CSV file is available in the expected location.
4. Open the notebook
`Analysis on TFL using Tube and Rail Stations in 2024-2025.ipynb`

Run all cells from top to bottom.

# Important File-Name Note
The notebook currently loads the following file name:
`StationFootfall_2024_2025 .csv`

## Repository Structure

```text
london-public-transport-footfall-analysis/
│
├── Analysis on TFL using Tube and Rail Stations in 2024-2025.ipynb
├── StationFootfall_2024_2025 .csv
├── README.md
└── LICENSE
````
## Ethics
This project uses secondary, publicly available, aggregated transport data. No human participants were recruited, contacted, surveyed, interviewed or observed.

The analysis is performed only at station level. Findings are interpreted carefully as potential demand patterns, disruption effects, holiday impacts or operational variation rather than evidence about individual passengers.

## Limitations
- The final study compares only two stations.
- Weather, school holidays, tourism events, strikes, engineering works and live disruption indicators were not included as direct model inputs.
-Forecast errors can increase during exceptional periods such as Christmas, service changes, closures or disruption.
- Results should not be interpreted as a network-wide TfL forecasting conclusion.

 ## Future Improvements
Future work could include:

- More stations and station categories
- Weather and holiday variables
- Real-time disruption indicators
- Separate entry and exit forecasting
- Longer forecasting horizons
- Rolling-origin evaluation
- Additional machine-learning and deep-learning benchmarks

## Author
Hari Raja Mohan

MSc Data Science Project

University of Hertfordshire

## License
This repository is released under the MIT Licence for project code and documentation only. TfL source data remains subject to its own terms and conditions.
