# **London Public Transport Footfall Analysis**

# Project Overview
This MSc Data Science project analyses daily public-transport footfall at two London stations between 1 January 2024 and 27 December 2025:
- King’s Cross St Pancras: a high-volume central interchange with mixed commuter, leisure and interchange demand.
- Roding Valley: a low-volume suburban station with a clearer commuter-led weekly pattern.

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
1.Load the station footfall dataset.
2.Clean column names, dates, station names, and tap-count values.
3. Create the target variable:
              Footfall = EntryTapCount + ExitTapCount   
4. Filter the data to King’s Cross St Pancras and Roding Valley.
5. Aggregate duplicate station-date records.
6. Reindex each station to a complete daily time series.
7. Impute missing observations using weekday-median imputation, time interpolation, and forward/backward completion.
8. Create calendar features including weekday, weekend flag, month, day, and ISO week number.
9. Perform exploratory data analysis, anomaly detection, forecasting, and model evaluation.

# Exploratory Data Analysis

The analysis includes:
1. Daily footfall trend plots
2. Seven-day rolling averages
3. Monthly average daily footfall plots
4. Weekday seasonality indices
5. Footfall distribution plots
6. Mean and median reference lines
7. Robust z-score anomaly visualisations
