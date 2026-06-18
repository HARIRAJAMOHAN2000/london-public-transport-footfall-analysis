# **London Public Transport Footfall Analysis**

# 1. Project Overview
This project analyses daily entry and exit footfall patterns at selected London public transport stations using Transport for London station-level tap-count data for 2024–2025. The main aim is to understand how passenger demand changes over time and to compare forecasting performance between a high-volume interchange station and a low-volume suburban station.

# 2. The project focuses on two stations:
- **King's Cross St Pancras** – a major central interchange with high passenger demand and greater exposure to disruption-sensitive movement.
- **Roding Valley** – a low-volume suburban station with a more regular commuter-led travel pattern.

The analysis combines **data cleaning, time-series preparation, exploratory data analysis, anomaly detection and forecasting** using both statistical and machine learning models.

# 3. Research Question
How do daily entry and exit footfall patterns in public transport stations change over time in 2024–2025?

# 4. Dataset
The dataset used in this project is based on Transport for London station footfall data. The original variables include:
- **TravelDate**
- **DayOfWeek**
- **Station**
- **EntryTapCount**
- **ExitTapCount**

## A new target variable was created:
                Footfall = EntryTapCount + ExitTapCount

The project uses station-level aggregated data only. No personal passenger-level data is used.

## Selected Stations

| Station                 | Station Type                | Reason for Selection                                                                |
| ----------------------- | --------------------------- | ----------------------------------------------------------------------------------- |
| King's Cross St Pancras | Major central interchange   | High demand, mixed passenger movement and disruption-sensitive behaviour.           |
| Roding Valley           | Low-volume suburban station | Smaller commuter-led demand pattern and useful contrast to King's Cross St Pancras. |

## Methodology
The analysis follows a structured data science workflow:

**1. Data loading and cleaning** 
- Loaded the TfL station footfall dataset.
- Cleaned column names and date formats.
- Converted entry and exit tap counts into numeric values.
- Created the total daily Footfall variable.

**2. Station filtering**
- Filtered the dataset to King's Cross St Pancras and Roding Valley.
  
**3. Daily time-series preparation**
- Combined duplicate station-date records.
- Reindexed each station into a complete daily time series.
- Filled missing values using weekday median imputation, time interpolation and forward/backward filling.
- Created calendar features such as weekday, weekend flag, month, day and ISO week number.

**4. Exploratory Data Analysis**
- Daily footfall trend and 7-day rolling average.
- Monthly average daily footfall.
- Average footfall by weekday.
- Distribution of daily footfall using mean and median reference lines.

**5. Anomaly detection**
- Used robust z-score anomaly detection based on the median and median absolute deviation.
- Compared anomaly behaviour between the two stations.

**6. Forecasting**
- Compared classical time-series models and machine learning models using a 90-day test period.


## Forecasting Models

**The following models were compared:**
- Seasonal Naive
- Holt-Winters Exponential Smoothing
- SARIMA
- SARIMAX with calendar features
- XGBoost with lag, rolling-window and calendar-based features

## Evaluation Metrics

**Forecasting performance was evaluated using:**
- MAE – Mean Absolute Error
- RMSE – Root Mean Squared Error
- MAPE – Mean Absolute Percentage Error
- sMAPE – Symmetric Mean Absolute Percentage Error

Using multiple metrics is important because the two selected stations operate at very different passenger scales.

## Key Findings

- King's Cross St Pancras had much higher average daily footfall and stronger volatility.
- Roding Valley had lower demand and a clearer weekday commuter pattern.
- King's Cross St Pancras had 12 detected anomalies, while Roding Valley had no detected anomalies at the selected threshold.
- XGBoost gave the strongest overall forecasting balance.
- For King's Cross St Pancras, XGBoost achieved the best MAE, MAPE and sMAPE, while Holt-Winters achieved the best RMSE.
- For Roding Valley, XGBoost achieved the best performance across all four metrics.
- The results show that model accuracy should be interpreted alongside station type, passenger scale and operational context.

## Repository Structure

```text
london-public-transport-footfall-analysis/
│
├── Analysis on TFL using Tube and Rail Stations in 2024-2025.ipynb
├── README.md
├── LICENSE
└── report/
    └── Data Science Final Report REF.docx
```


## How to Run the Notebook

1. Open the notebook in Google Colab or Jupyter Notebook.
2. Upload the TfL station footfall CSV file.
3. Check that the dataset path in the notebook matches the uploaded file location.
4. Run the notebook cells in order from data loading to final model evaluation.
5. Review the generated EDA plots, anomaly outputs, forecast comparison charts and model evaluation tables.

## Main Python Libraries

The project uses:
- pandas
- numpy
- matplotlib
- seaborn
- statsmodels
- scikit-learn
- xgboost

## Outputs

The notebook produces:
- Cleaned station-level daily time series
- EDA summary tables
- Daily trend and rolling average plots
- Monthly average footfall plots
- Weekday seasonality plots
- Footfall distribution plots
- Robust z-score anomaly plots
- Forecast comparison plots
- Model-minus-actual error visualisations
- Forecast metric tables
- Best model per station summary
- Overall model ranking
- License and Data Use

## License and Data Use

- The code in this repository is released under the MIT License.
- The original Transport for London dataset is not owned by this repository. The dataset should be used according to Transport for London's data service terms and conditions. The MIT Licence applies only to the project code, notebook structure and documentation, not to the original TfL data.

## Author
 Hari Raja Mohan  
 MSc Data Science Project  
 University of Hertfordshire
