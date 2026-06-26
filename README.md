<div align="center">

# ✈️ Airline Performance Analytics Dashboard
### Flight Delays & Cancellations 2019–2023 | Power BI Project

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Dataset](https://img.shields.io/badge/Dataset-Kaggle-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)
![Records](https://img.shields.io/badge/Records-3M%2B-red?style=for-the-badge)
![Pages](https://img.shields.io/badge/Report%20Pages-5-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

### 👤 Author

[![Author](https://img.shields.io/badge/Author-RENSEE%20GAJIPARA-blueviolet?style=for-the-badge&logo=github)](https://github.com/)
[![Institute](https://img.shields.io/badge/Institute-Red%20%26%20White%20Skill%20Education-CC0000?style=for-the-badge)](https://redandwhite.in/)
[![Subject](https://img.shields.io/badge/Subject-Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)]()

</div>

---

> 🌊 *"Data is the new oil — refined, it powers decisions at the speed of flight."*
> 
> 🌊 *This dashboard transforms 3 million raw flight records into a story of disruption, recovery, and performance — from the COVID-19 collapse of 2020 to the turbulent rebound of 2023.*

---

## 📋 Project Overview

This Power BI project analyzes **US domestic flight performance** from 2019 to 2023 using a real-world dataset sourced from the Bureau of Transportation Statistics (BTS). The dashboard covers the full arc of the aviation industry — the pre-pandemic boom, the COVID-19 collapse, and the gradual, uneven recovery.

| Field | Detail |
|---|---|
| **Institute** | Red & White Skill Education |
| **Subject** | Power BI |
| **Project** | Practical Report 5 (PR 5) |
| **Total Marks** | 10 |
| **Dataset** | Flight Delay and Cancellation Dataset (2019–2023) — Kaggle |

---

## 📸 Dashboard Preview

### 🖥️ Main Overview Page

![Dashboard Preview](Dashboard_Preview.png)

> The Overview page features KPI cards, a donut chart showing flight outcome distribution, a gauge measuring on-time rate vs. the 80% industry benchmark, and a stacked bar chart tracking yearly flight outcomes from 2019 to 2023.

---

### 🗺️ Custom Tooltip — Airport Detail

![Custom Tooltip](customized_Tooltip.png)

> Hovering over any airport bubble on the map triggers a custom tooltip page showing **Total Flights**, **Cancellation Rate%**, and a bar chart of the **Top 5 Destination Airports** — all dynamically filtered to the hovered airport.

---

## 📊 Dataset

| Field | Detail |
|---|---|
| **Dataset Name** | Flight Delay and Cancellation Dataset (2019–2023) |
| **Author** | Patrick Zelazko (Kaggle) |
| **Kaggle URL** | [Open Dataset ↗](https://www.kaggle.com/datasets/patrickzel/flight-delay-and-cancellation-dataset-2019-2023) |
| **Total Rows** | ~29 million (full) \| 3 million rows (sample used) |
| **Time Period** | 2019 to 2023 — 5 years of US domestic flights |
| **Source** | US Department of Transportation — Bureau of Transportation Statistics |

---

## 🛠️ Steps Performed

### ✅ Task 1 — Load Data & Power Query Preparation

- Loaded `flights_sample_3m.csv` via **Home → Get Data → Text/CSV → Transform Data**
- Set correct column data types: `FL_DATE` → Date, `DEP_DELAY` / `ARR_DELAY` → Decimal, `CANCELLED` / `DIVERTED` → Whole Number
- Extracted date parts: `Flight_Year`, `Flight_Month`, `Flight_DayOfWeek`, and `Flight_Month_Start` (continuous date for forecast)
- Replaced `CANCELLATION_CODE` values: `A → Carrier`, `B → Weather`, `C → National Air System`, `D → Security`
- Replaced `null` values in delay columns (`CARRIER_DELAY`, `WEATHER_DELAY`, `NAS_DELAY`, `SECURITY_DELAY`, `LATE_AIRCRAFT_DELAY`) with `0`
- Added custom column **Delay_Status** using:
  ```
  if [CANCELLED] = 1 then "Cancelled"
  else if [DIVERTED] = 1 then "Diverted"
  else if [ARR_DELAY] > 15 then "Delayed"
  else "On Time"
  ```
- Renamed query table to **FlightData** and closed & applied

---

### ✅ Task 2 — Dashboard Design Framework & Page Setup

- Sketched a **16:9 wireframe layout** before building visuals
- Set canvas to **1280 × 720px** (File → Options → Canvas Settings)
- Set canvas background to `#F5F5F5` (light grey), transparency 0%
- Enabled **Gridlines** and **Snap to Grid** (View tab)
- Created **5 report pages**: Overview | Airline Performance | Route & Airport Map | Drill-Through Detail | Trends & Forecast
- Added a **red header text box** (`#CC0000`, white 14pt bold) to every page with the format: `Flight Delays 2019-2023 - [Page Name] | Red & White Skill Education`

---

### ✅ Task 3 — DAX Measures & KPI Cards

Created all 6 measures in a dedicated **`_Measures`** table:

| Measure | DAX Formula | Purpose |
|---|---|---|
| `Total_Flights` | `COUNTROWS(FlightData)` | Baseline flight count |
| `Total_Cancelled` | `CALCULATE(COUNTROWS(FlightData), FlightData[CANCELLED]=1)` | Cancelled flight count |
| `Cancellation_Rate%` | `DIVIDE([Total_Cancelled],[Total_Flights])` | % flights cancelled |
| `Avg_Dep_Delay` | `AVERAGEX(FILTER(FlightData, FlightData[CANCELLED]=0), FlightData[DEP_DELAY])` | Avg departure delay (operated flights) |
| `Avg_Arr_Delay` | `AVERAGEX(FILTER(FlightData, FlightData[CANCELLED]=0), FlightData[ARR_DELAY])` | Avg arrival delay (operated flights) |
| `On_Time_Rate%` | `DIVIDE(COUNTROWS(FILTER(FlightData, FlightData[ARR_DELAY]<=15 && FlightData[CANCELLED]=0 && FlightData[DIVERTED]=0)), COUNTROWS(FILTER(FlightData, FlightData[CANCELLED]=0 && FlightData[DIVERTED]=0)))` | % flights on time (industry standard ≤15 min) |

- Built **4 KPI Cards** in the top row of the Overview page
- Added a **Multi-Row Card** as a compact summary panel

---

### ✅ Task 4 — Line Chart with Trend Line & Forecast

- Created a **Monthly Flight Volume Line Chart** (X: `Flight_Month`, Y: `Total_Flights`, Legend: `Flight_Year`) to visualize the COVID-19 collapse and recovery
- Added a **Trend Line** (red `#CC0000`, dashed) via the Analytics panel
- Built a separate **Forecast Chart** using `Flight_Month_Start` (continuous date axis) with a **6-month forecast** at 95% confidence interval
- Added a **Monthly Avg Departure Delay** line chart by year
- Formatted both charts: removed gridlines, added data labels, white background, source subtitle

---

### ✅ Task 5 — Advanced Chart Types

| Visual | Configuration | Page |
|---|---|---|
| Clustered Bar — Top 10 Airlines by Cancellations | Top N filter, red bars, data labels | Airline Performance |
| Clustered Bar — Avg Delay by Airline | Top 10 by `Avg_Dep_Delay` | Airline Performance |
| Donut — Cancellation Reason | Legend: `CANCELLATION_CODE`, Values: `Total_Cancelled` | Overview |
| Donut — Flight Outcome Distribution | Legend: `Delay_Status`, Values: `Total_Flights` | Overview |
| Matrix — Cancellation Rate Heatmap | Rows: Airline, Columns: Year, Values: `Cancellation_Rate%` with conditional formatting | Airline Performance |
| Gauge — On-Time Rate vs 80% Target | Value: `On_Time_Rate%`, Target: 0.80, needle: red `#CC0000` | Overview |

---

### ✅ Task 6 — Conditional Formatting, Top-N Filters & Slicers

- Created **`Delay_Category`** calculated column using `SWITCH(TRUE(), ...)` — checks Cancelled and Diverted first, then Major Delay (>60 min), Minor Delay (15–60 min), On Time
- Applied **conditional formatting heatmap** to Matrix (White → Red gradient)
- Applied **icon sets** to Table visual: green circle / yellow triangle / red X based on `Cancellation_Rate%` and `On_Time_Rate%` thresholds
- Applied **Top-N filters** to both bar charts (Top 10 by cancellations and by avg delay)
- Added **4 slicers**: `Flight_Year` (Tile), `AIRLINE` (Dropdown), `Flight_Month` (Between slider), `Delay_Status` (List)

---

### ✅ Task 7 — Map Visual

- Enabled Map visuals: **File → Options → Security → Use Map and Filled Map visuals**
- Built **Bubble Map** (Location: `ORIGIN`, Size: `Total_Flights`, Colour: `Avg_Dep_Delay`) — larger = more flights, darker = higher delay
- Built **Filled Map** (Location: State, Colour: `Cancellation_Rate%`) — darker state = higher cancellation rate
- Formatted both maps: Road style (Bubble), Light style (Filled), labels on, synced `Flight_Year` slicer
- Assigned **Custom Tooltip page** (`Tooltip_Airport`) to the Bubble Map

---

### ✅ Task 8 — Drill-Through: Airline Detail Page

- Configured **Drill-Through** on the Drill-Through Detail page by dragging `AIRLINE` into the drill-through fields zone
- Added visuals: KPI Cards (Total Flights, Cancellation Rate), Monthly Cancellations bar chart, Cancellation Reason donut, On-Time Rate trend line, flight details table (Top 100 rows)
- Tested drill-through: right-click airline bar → Drill Through → verified page filters to that airline
- Styled **Back button**: red `#CC0000` fill, white bold font, labelled *"Back to Airline Performance"*
- Demonstrated **Drill Down** on the Matrix (single-arrow vs. double-arrow expand)

---

### ✅ Task 9 — Bookmarks & Custom Navigation Bar

- Created **5 navigation buttons** per page (Overview | Airlines | Map | Detail | Trends) using Insert → Buttons → Blank, with Page Navigation actions
- Formatted nav bar: red `#CC0000` fill, white 10pt bold, dark red `#990000` hover state, Format Painter used for consistency
- Grouped Year + Airline + Delay_Status slicers as **SlicerPanel**
- Created **Bookmark pair**: `Slicer_Visible` and `Slicer_Hidden` with Show/Hide Filters toggle buttons at the same position
- Created **Numeric Range Parameter** (`Min_Delay_Minutes`, 0–120, step 15) and measure `Flights_Above_Min_Delay` wired to a KPI Card on the Trends page

---

### ✅ Task 10 — Fields Parameters, Custom Tooltips & Mobile Layout

- Created **Fields Parameter** (`Metric_Selector`) with: Total_Flights, Total_Cancelled, Avg_Dep_Delay, Avg_Arr_Delay, On_Time_Rate%
- Connected to a **dedicated Airline Comparison bar chart** (separate from the Top 10 chart to avoid Top-N conflict)
- Edited **Report Interactions**: Gauge set to *None* for the `Flight_Year` slicer (benchmark stays constant)
- Built **Custom Tooltip page** (`Tooltip_Airport`): KPI cards + Top 5 Destinations bar chart, assigned to Bubble Map
- Configured **Mobile Layout** on Overview page: Multi-Row Card → Outcome Donut → Cancellation Cause Donut → Airline Slicer
- Applied built-in **theme** and performed final alignment (Align Top, Distribute Horizontally) across all 5 pages

---

## 🗂️ Report Pages

| # | Page | Key Visuals |
|---|---|---|
| 1 | **Overview** | KPI cards, Gauge, Donut charts, Slicer panel, Fields Parameter chart |
| 2 | **Airline Performance** | Top 10 bar charts, Matrix heatmap, Conditional formatting table |
| 3 | **Route & Airport Map** | Bubble Map, Filled Map, Custom tooltip |
| 4 | **Drill-Through Detail** | Per-airline KPIs, Monthly bar chart, Cancellation donut, Flight table |
| 5 | **Trends & Forecast** | Line charts, Trend line, 6-month forecast, Min Delay parameter |

---

## 🧰 Tools Used

![Power BI Desktop](https://img.shields.io/badge/Power%20BI%20Desktop-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-0078D4?style=flat-square&logo=microsoft&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-CC0000?style=flat-square&logo=microsoft&logoColor=white)
![Kaggle](https://img.shields.io/badge/Kaggle-20BEFF?style=flat-square&logo=kaggle&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)

---

## 🎬 Video Walkthrough

> 📹 *Add your video link here:*

[![Watch Demo](https://img.shields.io/badge/Watch%20Demo-YouTube-FF0000?style=for-the-badge&logo=youtube)](YOUR_VIDEO_LINK_HERE)

---

## 📁 Repository Structure

```
📦 Airline-Performance-Analytics-Dashboard
 ┣ 📂 screenshots
 ┃ ┣ 🖼️ Dashboard_Preview.png
 ┃ ┣ 🖼️ customized_Tooltip.png
 ┃ ┗ 🖼️ (add all 5 page screenshots here)
 ┣ 📊 Airline_Performance_Analytics_Dashboard.pbix
 ┣ 📄 README.md
 ┗ 🔗 Dataset: https://www.kaggle.com/datasets/patrickzel/flight-delay-and-cancellation-dataset-2019-2023
```

---

<div align="center">

🌊 *"Every delayed flight tells a story. This dashboard tells them all."* 🌊

---

Made with ❤️ by **RENSEE GAJIPARA** · Red & White Skill Education

![Visitors](https://img.shields.io/badge/Power%20BI-PR5-CC0000?style=flat-square)

</div>
