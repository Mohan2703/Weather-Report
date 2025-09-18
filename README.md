# ⛅ Weather Monitoring & Air Quality Dashboard - Power BI

[![View Portfolio](https://img.shields.io/badge/View%20Portfolio-%23000000.svg?style=for-the-badge&logo=firefox&logoColor=#FF7139)](https://www.datascienceportfol.io/mohan_Srinivas)
[![View Dashboard](https://img.shields.io/badge/View%20Dashboard-%23000000.svg?style=for-the-badge&logo=Codeforces&logoColor=gold)](https://app.powerbi.com/view?r=eyJrIjoiMDMyZjU4NGMtMTFlMi00ZDBhLWIwNGEtODZkMWM2OTljNzE5IiwidCI6IjM3MzhkYjE5LTA4MzUtNDhmZS05MjhiLWMxZjI3ZmNkN2Y2NCJ9)

## Table of Contents
- [Problem Statement](#problem-statement)
- [Project Planning using Star Method](#project-planning-using-star-method)
- [Data Source](#data-source)
- [Data Preprocessing \& ETL](#data-preprocessing--etl)
- [Data Modelling](#data-modelling)
- [Data Analysis](#data-analysis)
- [Dashboard](#dashboard)
- [Findings](#findings)
- [Tools And Softwares](#tools-and-softwares)


## Problem Statement
<details>
<summary>
$\textsf{\color{blue}{View Problem Statement ➡️}}$
</summary><br>

Access to consolidated, accurate weather and air quality data is critical for urban residents, policy makers, and planners. However, this information is often fragmented across sources, lacks actionable health context, and is not available in an interactive, real-time analytics dashboard.
- **The Problem:**
How can diverse weather and AQI variables be synthesized into a unified dashboard for rapid decision-making, public awareness, and urban planning?
</details>


## Project Planning using Star Method
<details>
<summary>
$\textsf{\color{blue}{View Stratergy ➡️}}$
</summary><br>

- **Understand key KPIs:** Total Engagement, Views, Impressions, Click-Through Rate (CTR), and Engagement Rate.
- **Build hierarchical view:** Platform → Content Category → Post Type → Post-Level Details.
- **Enable drilldowns:** from a high-level overview → campaign analysis → regional performance → content-specific insights.
- **Design dashboards:** clear filters for platform, country, campaign, content type, and date range.

### 📝 S - Situation
- Agencies and urban stakeholders required live, context-rich weather and AQI insights for multiple Indian cities, but data was fragmented, non-intuitive, and lacked health-centric interpretations.

### 🎯 T - Task
- Design and deliver an interactive Power BI dashboard with multi-city weather reporting that provides real-time and forecasted weather and AQI metrics,  incorporates environmental health advisories, and leverages robust data modeling for high-fidelity analytics.

### ⚡ A - Action
- Imported weather and AQI data streams for cities like Bengaluru, Chennai, Hyderabad, Mumbai, Panaji, and Thiruvananthapuram.
- Cleaned, transformed, and Built a star schema data model with key relationships (city, date/time)
- Defined calculated columns for day names, daylight duration, formatting sunrise/sunset, and other contextual metrics.
- Built DAX measures for AQI status, suggestions, dynamic tile coloring, and weather summaries.
- Designed a modular dashboard: city selection, weather/AQI summary, forecast visualization, sunrise/sunset, rain probability, and data freshness.

### 🏆 R - Result
- Delivered a comprehensive, intuitive dashboard that informs users of weather trends, air quality conditions, and recommended actions.
- Enabled data-driven decisions for public advisories and event planning.
- The modular structure allows easy integration of new metrics, locations, or pollutant types.
</details>


## Data Source
<details>
<summary>
$\textsf{\color{blue}{View Data Source ➡️}}$
</summary><br>

>- **Datasource:** Weather API: JSON data fetched from [weatherapi.com](https://api.weatherapi.com/v1/forecast.json) for cities for **3 days** with **aqi**
>- Excel File (Bg_Img.xlsx): containing a table that maps weather conditions to background images.
>- Primary: Live weather and air quality APIs covering Indian metro cities
>- Metrics: Temperature, humidity, wind, pressure, visibility, UV index, precipitation, PM10, PM2.5, CO, SO2, O3, NO2, sunrise/sunset
</details>


## Data Preprocessing & ETL
<details>
<summary>
$\textsf{\color{blue}{View PreProcssing Steps ➡️}}$
</summary><br>

**Raw weather data was imported from the WeatherAPI into Power BI, and the following ETL process was executed in Power Query:**

#### 🗂 Master data Preparation 
1. Connected to the WeatherAPI JSON endpoint for each city.
2. Expanded nested JSON objects (location, current, forecast → day, astro, hour, condition, air_quality).
3. Applied appropriate data types to columns (e.g., type text for strings, Int64.Type for integers, type number for decimals, type datetime for timestamps).
4. Replaced // with https:// in the both forecast.hour and date Condition.Icon field to fix broken image URLs.
5. Combined individual city tables into a single Master table using Table.Combine for unified processing.
6. Created a Master table as the base for all further reference queries.

#### 🌞 Daily Forecast ETL
1. Referenced the Master table.
2. Removed unnecessary columns such as Hourly details and Current conditions.
3. Applied deduplication on {Location, ForecastDate} to ensure unique daily records per city.

#### ⏰ Hourly Forecast ETL
1. Referenced the Master table.
2. Expanded hourly forecast records (forecast.forecastday.hour).
3. Created separate Date and Hour columns by splitting the timestamp.
4. Removed duplicate/unnecessary columns not relevant to hourly analysis.

#### 🌍 Current Weather ETL
1. Referenced the Master table.
2. Removed forecast and hourly columns, retaining only current weather snapshot per city.
3. Cleaned nulls and ensured correct data types for numeric and text fields.

#### 🖼️ Dynamic Weather Changing Background
1. Loaded and transformed an Excel file (Bg_Img.xlsx), setting Weather and Bg columns as type text to map weather conditions to background images.
</details>


## Data Modelling
<details>
<summary>
$\textsf{\color{blue}{View Modelling ➡️}}$
</summary><br>

<img width="600" height="600" alt="Image" src="https://github.com/user-attachments/assets/baf7fcba-e41f-4376-ba34-5603da120728" /> <br> 
The data model in Power BI was designed to connect fact tables (live, daily, hourly) with supporting dimension and helper tables, enabling flexible and dynamic weather analysis.

- **Tables Used:**
  - **Current** (Fact Table) → Central table for live weather and air quality measures (temperature, humidity, pollutant levels) by city and timestamp.
  - **Forecast_Day** (Fact Table) → Day-level forecasted weather per city, including temperatures, rain chances, and astro data (sunrise/sunset).
  - **Forecast_Hour** (Fact Table) → Hour-level forecasted weather, enabling granular trend analysis and detailed visualizations.
  - **Master** (Fact Table) → Integrates metadata for multi-source refresh and time-point validation.
  - **Weather_Bg** (Dimension Table) → Maps weather conditions to dashboard backgrounds, ensuring visuals adapt dynamically to real conditions.
  - **Measuress** (Helper Table) →  A disconnected table created specifically to group and organize all DAX measures.
  - **City-specific tables** (Dimension Tables) → Extend easy multi-location tracking, promoting modularity and separation of city datasets.

- **Time Intelligence:**
  -  Dedicated Date tables connected to timestamps across Current, Forecast_Day, Forecast_Hour, and Data_Refresh.
  - Enables analysis by day of week, month, custom periods, and precise filtering for historical vs. forecast trends.
- **Relationship Setup:**
  - **One-to-Many** link Current with Forecast_Day and Forecast_Hour on city and date/time.
  - **Bi-directional relationship** between [Weather_Bg] and [Current] allows real-time condition-driven background changes.
  - City, day, and hour tables are synchronized to support drill-downs from overview → daily → hourly weather insights.
</details>


## Data Analysis
<details>
<summary>
$\textsf{\color{blue}{Power BI: View Created Dax Measures, Columns, Tables ➡️}}$
</summary><br>

**Measures:**
1. AQI Suggestion
```
VAR AQI  = SELECTEDVALUE('Current' [current.air_quality.pm10])
RETURN
SWITCH(
TRUE(),
AQI <= 50, "Air is clean and healthy",
AQI <= 100, "Acceptable air quality, stay active",
AQI <= 150, "Sensitive groups should reduce outdoor time",
AQI <= 200, "Limit prolonged outdoor exertion",
AQI <= 300, "Avoid outdoor activity if possible",
"Stay indoors, wear mask if outside"
)
```
2. AQI_Status
```
VAR AQI = ROUND(SELECTEDVALUE('Current'[current.air_quality.pm10]),0)
RETURN 
SWITCH(
    TRUE(),
    AQI <= 50, "Good",
    AQI <= 100, "Moderate",
    AQI <= 150, "Unhealthy For Sensitive",
    AQI <= 200, "Unhealthy",
    AQI <= 300, "Very Unhealthy",
    "Hazardous"
)
```
3. Wind_Speed
```
SUM('Current'[current.wind_kph])&" Kph"
```
4. Last_Updated_Date_Curr
```
"Last Updated, "&FORMAT(FIRSTNONBLANK('Current'[current.last_updated],""),"dd mmm")
```
5. Left_value_Rain
```
100 - SUM(Forecast_day[forecast.forecastday.day.daily_chance_of_rain])
```
6. Curr_Temp_C & Forecast_Temp_C
```
1. SUM('Current'[current.temp_c])&" °C"
2. ROUND(AVERAGE(Forecast_day[forecast.forecastday.day.avgtemp_c]), 1) & " °C"
```

**Calculated Columns:**
1. Day_Name 
```
FORMAT(Forecast_day[forecast.forecastday.date], "dd dddd")
```
2. DaylightDuration
```
VAR Minutes = DATEDIFF(Forecast_day[Sunrise], Forecast_day[Sunset], MINUTE)
VAR Hours = INT(Minutes / 60)
VAR Mins = MOD(Minutes, 60)
RETURN
Hours & " hrs " & Mins & " mins"
```
3. Sunrise & Sunset
```
1. FORMAT(Forecast_day[forecast.forecastday.astro.sunrise],"hh:mm AM/PM")
2. FORMAT(Forecast_day[forecast.forecastday.astro.sunset],"hh:mm AM/PM")
```
</details>


## Dashboard
<details>
<summary>
$\textsf{\color{blue}{View Images ➡️}}$
</summary><br>

> ### 1. Weather Report
> <a href="https://app.powerbi.com/view?r=eyJrIjoiMDMyZjU4NGMtMTFlMi00ZDBhLWIwNGEtODZkMWM2OTljNzE5IiwidCI6IjM3MzhkYjE5LTA4MzUtNDhmZS05MjhiLWMxZjI3ZmNkN2Y2NCJ9" target="_blank"> <img width="650" height="400" alt="Image" src="https://github.com/user-attachments/assets/f2f28ae0-46ff-4338-ab92-4ddea982bb0d" /></a>
</details>


## Findings
<details>
<summary>
$\textsf{\color{blue}{View Findings ➡️}}$
</summary><br>

- Single-point user interface for real-time and forecasted weather and AQI across major Indian cities.
- Interactive visuals highlight city-level trends in temperature, humidity, wind, and pollutants for quick comparisons.
- Color-coded AQI and pollutant tiles (e.g., green for safe, red for hazardous) enable rapid risk assessment.
- Displays daylight duration, sunrise/sunset, and chance of rain for effective planning.
- Automated scheduled refresh ensures data is always current, building user trust.
- Dynamic background changes based on real-time weather conditions, enhancing the visual experience.
</details>


## Tools And Softwares
<details>
<summary>
$\textsf{\color{blue}{View Tools ➡️}}$
</summary><br>

- **Power BI** → Data model, DAX, dashboard dev
- **Source Data** → Weather/AQI public APIs
- **Modeling** → Advanced DAX, calculated columns, custom color models
- **Excel/CSV** → Raw dataset handling
- **Icons/Images** → For visual representation in dashboard
</details>
