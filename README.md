# London Public Transport Footfall Analysis

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

# A new target variable was created:
                Footfall = EntryTapCount + ExitTapCount

The project uses station-level aggregated data only. No personal passenger-level data is used.
