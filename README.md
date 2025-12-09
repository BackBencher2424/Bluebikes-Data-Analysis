# Bluebikes-Data-Analysis

# 🚲 Bluebikes Boston (Jan–Sep 2025): Weather-Aware Demand, Forecasting & Rebalancing 🌦️

> **Course:** BA775 – Fall 2025, Section A06  
> **Notebook:** `A06-Bluebikes-Weather-Forecasting.ipynb`  
> **Team:** Emily Su · Vishesh Goyal · Grace Kung · Mokhinur Talibzhanova · Anton Falk  

---

## 📌 Project Overview

Boston’s Bluebikes system sees big swings in ridership across seasons, hours of the day, and especially weather conditions.  
This project combines **Bluebikes trip data (Jan–Sep 2025)** with **hourly weather data** to:

- Understand how **stations, routes, and riders** behave under different weather and time-of-day conditions.  
- Build **weather-aware demand indicators** to support **bike rebalancing** and reduce the risk of **empty/full stations**.  
- Provide **actionable recommendations** for operations teams (where to add capacity, when to move bikes, and which stations are weather-resilient).

The core analysis is implemented in a single, well-documented notebook:  
`A06-Bluebikes-Weather-Forecasting.ipynb`.

---

## 🔍 Key Questions We Answer

The notebook is structured around a set of guiding questions, including (but not limited to):

1. **Which stations have the highest number of departures and returns?**  
2. **What are the most popular start/end station routes?**  
3. **How do peak usage hours differ, and how does ridership vary across months?**  
4. **How does average trip duration differ between members and casual riders, and what does that imply for availability?**  
5. **How does weather (temperature, rain, wind) influence total trips and demand patterns?**  
6. **How does weather influence AM vs. PM peak usage differently?**  
7. **Does wind speed actually slow riders (distance/duration), or just reduce total demand?**  
8. **Which stations have the most stable demand regardless of weather (weather-resilient “backbone” stations)?**  
9. **Which stations consistently lose bikes in the morning and gain them back in the evening (rebalancing hotspots)?**  
10. **How elastic is demand with respect to rain and temperature (% change in trips per unit change in weather)?**

Each question is backed by BigQuery SQL, visualizations, and a short **“So what?”** interpretation explaining the operational impact.

---

## 🧱 Data Sources

All data is accessed via **Google BigQuery** (schema and column descriptions are documented in the notebook).

### 1. Bluebikes Trip Data (Jan–Sep 2025)

- Monthly tables under a dataset similar to:  
  `ba775-fall25-a06.bluebike_weather.2025MM-bluebikes-tripdata`
- Key columns include:
  - `date` – ride date  
  - `ride_id` – unique ride identifier  
  - `rideable_type` – classic vs. electric  
  - `started_at`, `ended_at` – start and end timestamps  
  - `start_station_name`, `start_station_id`  
  - `end_station_name`, `end_station_id`  
  - `start_lat`, `start_lng`, `end_lat`, `end_lng`  
  - `member_casual` – rider type (member or casual)

### 2. Weather Data (Hourly)

- Joined to trips at the **date / approximate hour** level.
- Typical fields:
  - `tempmax`, `tempmin`, `temp`  
  - `precip`, `precipprob`  
  - `windspeed`, `windgust`  
  - `conditions` / derived **weather category**

The notebook includes a **table dictionary** describing each field and the merged dataset schema.

---

## 📊 Dashboards & Visual Storytelling

The analysis is designed to support interactive dashboards (built in Tableau) that answer:

- **Q1 & Q2:** Station demand & route popularity  
- **Q3 & Q4:** Trip duration vs. rider type, hourly & monthly peaks  
- **Q5–Q10:** Weather-sensitive demand, elasticities, and station resilience  

> You can link your Tableau Public dashboards here, e.g.:  
> `https://public.tableau.com/...`  

The notebook also embeds screenshots of final dashboards and stories for reference.

---

## 🧮 Analytical Approach

High-level workflow inside the notebook:

1. **Data Engineering & Merging**
   - Load Bluebikes monthly trip tables from BigQuery.
   - Join with hourly weather data into a single **Bluebikes + Weather** fact table.
   - Standardize station identifiers and ensure consistent date/time handling.

2. **Exploratory Data Analysis (EDA)**
   - Station-level demand (departures vs. returns).
   - Route-level flows (A→B vs. B→A comparisons).
   - Member vs. casual behavior (trip duration, days of week, seasonality).
   - Demand over time (hour of day, day of week, month).

3. **Weather-Aware Metrics & Indices**
   - Compare normal vs. rain vs. extreme heat days.
   - Derive **weather sensitivity** for each station (how much demand drops under bad weather).
   - Examine **wind vs. speed** (distance/duration) to see whether riders slow down or simply ride less.
   - Build elasticity views such as:  
     > % change in trips per inch of rain / per degree of temperature change.

4. **Rebalancing Insights**
   - Identify stations that **consistently lose bikes in the morning and gain them back in the evening**.
   - Highlight **weather-resilient stations** that keep demand stable even when conditions change.
   - Suggest rebalancing strategies:
     - Prioritize high-impact central stations.
     - Use weather sensitivity to plan truck routes and bike redistribution.
     - Focus on corridors where under-supply or over-supply is most common.

---

## 🗂️ Repository Structure

Suggested structure for this repo:

```text
.
├── A06-Bluebikes-Weather-Forecasting.ipynb   # Main analysis & narrative
├── README.md                                 # You are here 🙂
├── img/                                      # Exported dashboard screenshots (optional)
│   ├── dashboard1.png
│   └── dashboard2.png
└── sql/                                      # Optional: saved BigQuery SQL scripts
    ├── station_demand.sql
    ├── weather_join.sql
    └── elasticity_metrics.sql
