# TravelTide Analytics — Customer Behavioural Segmentation & Retention

## 📋 Table of Contents

1. [Project Title & Description](#1-project-title--description)
2. [Executive Summary](#2-executive-summary)
3. [Project Summary](#3-project-summary)
4. [Directory Structure](#4-directory-structure)
5. [Installation Instructions](#5-installation-instructions)
6. [Usage Instructions](#6-usage-instructions)
7. [How to Extract the ~49,000 Filtered Records](#7-how-to-extract-the-49000-filtered-records)
8. [CSV File — User Perk Assignments](#8-csv-file--user-perk-assignments)
9. [Dependencies](#9-dependencies)
10. [Visualizations](#10-visualizations)
11. [Assessment Checklist](#11-assessment-checklist)

## 1. Project Title & Description

### TravelTide: Customer Behavioural Segmentation & Rewards Perk Assignment

TravelTide is a travel booking platform that experienced rapid user growth in early 2023, followed by a sustained decline in Daily Active Users. This project addresses that decline by:

- **Building a unified session-level dataset** joining sessions, users, flights, and hotels from the platform's Databricks Delta tables
- **Performing exploratory data analysis (EDA)** on both session-level and user-aggregated data to surface actionable business insights
- **Segmenting users** using a traditional RFM (Recency, Frequency, Monetary) scoring approach
- **Assigning every user to exactly one rewards perk** from a defined set of five options based on their behavioural profile
- **Testing perk effectiveness** through a controlled A/B experiment tracking 90-day retention rates
- **Delivering strategic recommendations** for the next perk programme iteration

The analysis focuses on a specific cohort: **users who had more than 7 sessions after January 4, 2023** — producing approximately **49,000 session records** and **~5,998 unique users**.


## 2. Executive Summary

📄 **[Read the full Executive Summary →]([docs/TravelTide_Executive_Summary.md](https://github.com/sonalibiswas24/TravelTide-Customer-Retention-Analysis/blob/main/presentation/TravelTide_Executive_Summary.md))**

### TL;DR

| Metric | Finding |
|---|---|
| Target cohort | Users with >7 sessions after Jan 4, 2023 |
| Session records analysed | ~49,000 |
| Unique users in cohort | ~5,998 |
| High-engagement conversion rate | **65%+** (users with 25+ clicks) |
| Flight-only booking gap | **75%** of sessions — biggest revenue leak |
| Peak engagement window | **17:00–21:00** daily |
| A/B test result | **Neutral** — segmentation is correct, perk design needs redesign |
| Primary recommendation | Fix experimentation window + redesign perk incentives |

## 3. Project Summary

### 3.1 Business Requirement

TravelTide needed to bridge the gap between raw clickstream data and a personalized marketing strategy. The specific requirement was to:

> *Analyze user session behaviour and assign every user in the target cohort to exactly one rewards perk — mapped to their actual travel habits — so that marketing communications are clear, targeted, and measurable.*

### 3.2 Goal

> *Build a customer segmentation model using behavioural features (session activity, booking rates, discount usage, cancellation history, demographics) and assign users to one of five perks using an RFM-based priority framework.*

### 3.3 Key Insights

#### Session-Level (Cell 50 → ~49,000 rows)
- **DAU peaked in April 2023** and has declined steadily — signalling post-launch engagement retraction
- **65%+ conversion rate** for users with 25+ clicks per session — funnel is healthy at depth
- **75% of flight sessions** have no hotel attached — cross-sell gap = major revenue opportunity
- **Deep-search sessions** (10+ clicks) produce both the most bookings AND the most cancellations — intent is high, intervention timing is everything
- **17:00–21:00** is the peak window for sessions, bookings, and revenue simultaneously

#### User-Level (Cell 108 → ~5,998 users)
- **67% of users have no children** — platform skews toward independent travellers and couples
- **85–90% US users are female** — strong brand resonance with women; male segment largely untapped
- **Top 3 home airports: LGA, JFK, LAX** — hyper-local campaigns to NYC + LA maximise ROAS
- **January 2023 cohort** (~3,500 users) was driven by the Winter Sale Campaign — the most strategic group for re-engagement

#### RFM Segmentation
| Segment | Count | Key Characteristic |
|---|---|---|
| Engaged | ~5,439 | High monetary value or moderate session depth |
| Frequent | ~9 | Very high session frequency |
| Other | ~550 | Low engagement baseline |

#### Perk Assignment
| Perk | Rule | Priority |
|---|---|---|
| Flight Discount | ≥2 flights + avg discount >$50 | 1st |
| Hotel Discount | ≥2 hotels + avg discount >$50 | 2nd |
| Family Perk | Has children + avg nights ≥3 | 3rd |
| Loyalty Perk | ≥10 sessions created | 4th |
| New User Perk | Sign-up ≥ Jan 2026 | 5th |
| Other | No condition met | Default |

### 3.4 Links to Deliverables

| Resource | Link |
|---|---|
| 📓 Databricks Notebook | [`notebooks/Traveltide_MPDS_7_1.ipynb`]((https://github.com/sonalibiswas24/TravelTide-Customer-Retention-Analysis/blob/main/notebooks/Traveltide%20MPDS%20(8).zip)) |
| 📊 Stakeholder Presentation | [`presentation/TravelTide_Analysis.pptx`](https://github.com/sonalibiswas24/TravelTide-Customer-Retention-Analysis/blob/main/presentation/TravelTide_Analysis.pptx)) |
| 📄 Executive Summary | [`docs/TravelTide_Executive_Summary.md`]((https://github.com/sonalibiswas24/TravelTide-Customer-Retention-Analysis/blob/main/presentation/TravelTide_Executive_Summary.md)) |
| 🗃️ User Perk CSV | [`data/traveltide_user_perk_assignments.csv`](https://github.com/sonalibiswas24/TravelTide-Customer-Retention-Analysis/blob/main/data/traveltide_user_perk_assignments.csv) ||

## 4. Directory Structure

```
traveltide-analytics/
│
├── README.md                          ← You are here
│
├── docs/
│   └── TravelTide_Executive_Summary.md   ← Executive summary document
│
├── notebooks/
│   └── Traveltide_MPDS_7_1.ipynb         ← Main Databricks analysis notebook
│
├── presentation/
│   └── TravelTide_Analysis.pptx          ← Stakeholder slide deck (24 slides)
│
├── data/
│   └── traveltide_user_perk_assignments.csv  ← Users with assigned perks (output)
│
├── sql/
│   ├── 01_sessions_filtered.sql          ← Filter logic for ~49,000 records
│   ├── 02_user_agg_features.sql          ← User aggregation table
│   └── 03_perk_assignment.sql            ← Perk assignment query
│
└── requirements.txt                   ← Python dependencies
```

> **Note:** The notebook runs inside **Databricks**. The `sql/` folder contains the key SQL queries extracted from the notebook for reference and reuse outside of the Databricks environment.


## 5. Installation Instructions

### Option A — Run on Databricks (Recommended)

This project was built on **Databricks Community Edition or Workspace** using Delta Lake. No local installation is required for the core analysis.

1. **Import the notebook into Databricks:**
   ```
   Databricks Workspace → Import → Upload File → Select Traveltide_MPDS_7_1.ipynb
   ```

2. **Attach a cluster** with the following runtime:
   ```
   Databricks Runtime 13.3 LTS or higher
   Python 3.10+, Spark 3.4+
   ```

3. **Ensure source tables exist** in your Databricks catalog:
   ```
   sessions_aggregate    ← Platform session log
   users_aggregate       ← User profile data
   flights_spark     ← Flight booking details
   hotels_spark      ← Hotel booking details
   ```

4. **Run all cells** — the notebook creates all intermediate Delta tables automatically.

### Option B — Run Locally (Python / Pandas only, no Spark)

If you only need to work with the **CSV output** (user perk assignments) without Databricks:

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/traveltide-analytics.git
cd traveltide-analytics

# 2. Create a virtual environment
python -m venv .venv
source .venv/bin/activate       # macOS/Linux
.venv\Scripts\activate          # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Open the data
python
>>> import pandas as pd
>>> df = pd.read_csv("data/traveltide_user_perk_assignments.csv")
>>> print(df.head())
```

## 6. Usage Instructions

### Step-by-Step: Running the Full Pipeline on Databricks

The notebook is structured in sequentially numbered sections. Run cells in order:

| Cells | Section | What it does |
|---|---|---|
| 1–35 | Setup & Exploration | Connect to source tables, initial row counts, data type checks |
| 36–51 | **Session State Table** | Join sessions + users + flights + hotels; apply cohort filter → `sessions_filtered` (~49,000 rows) |
| 52–105 | **Session EDA** | 5+ analytical plots: DAU trend, engagement vs conversion, bundle gap, search depth, hourly patterns |
| 106–108 | **User Aggregated Table** | Aggregate `sessions_filtered` to one row per user → `user_agg_features` (~5,998 rows) |
| 109–135 | **User EDA** | Sign-up trends, cohort analysis, demographics, discount impact, booking funnel |
| 117–119 | **RFM Segmentation** | Score each user on R, F, M → assign segment label → treemap visualisation |
| 120–125 | **Perk Assignment & A/B Test** | Assign perks by priority rules; split into Control/Treatment; run chi-square test |
| 126–135 | **Monitoring & Results** | Monthly retention tracking by cohort and perk type |
| 136–141 | **Final Perk Output** | `user_features_pd` DataFrame with `user_id` and `assigned_perk` columns |

### Exporting the Final Perk Assignments from Databricks

After running all cells, export the result to CSV:

```python
# In a Databricks notebook cell:
user_features_pd[['user', 'assigned_perk']].to_csv(
    '/dbfs/FileStore/traveltide_user_perk_assignments.csv',
    index=False
)
```

Then download from Databricks:
```
Databricks UI → Data → DBFS → FileStore → traveltide_user_perk_assignments.csv → Download
```

## 7. How to Extract the ~49,000 Filtered Records

This is the **most important step** in the pipeline. The `sessions_filtered` table is the foundation of all downstream analysis.

### The Filter Logic (Cell 50 in the notebook)

The ~49,000 records come from applying **two sequential filters**:

```sql
-- ============================================================
-- STEP 1: Filter sessions to only those AFTER January 4, 2023
-- STEP 2: Keep only users who had MORE THAN 7 such sessions
-- STEP 3: Join with users, flights, and hotels for full context
-- Result: ~49,000 session rows covering ~5,998 unique users
-- ============================================================

CREATE OR REPLACE TABLE workspace.default.sessions_filtered
USING DELTA
AS
WITH sessions_2023 AS (
  -- Filter 1: Only sessions after the cohort start date
  SELECT *
  FROM sessions_spark
  WHERE session_start > '2023-01-04'
),

filtered_users AS (
  -- Filter 2: Only users with more than 7 qualifying sessions
  -- This removes one-time visitors and ensures minimum engagement depth
  SELECT
    user_id,
    COUNT(*) AS session_count
  FROM sessions_2023
  GROUP BY user_id
  HAVING COUNT(*) > 7          -- <-- THIS is the key threshold
),

session_base AS (
  -- Final join: bring in all context columns
  SELECT
    s.session_id,
    s.user_id,
    s.trip_id,
    s.session_start,
    s.session_end,
    s.page_clicks,
    s.flight_discount,
    s.flight_discount_amount,
    s.hotel_discount,
    s.hotel_discount_amount,
    s.flight_booked,
    s.hotel_booked,
    s.cancellation,
    -- User profile
    u.birthdate, u.gender, u.married, u.has_children,
    u.home_country, u.home_city, u.home_airport,
    u.home_airport_lat, u.home_airport_lon,
    u.sign_up_date,
    -- Flight details
    f.origin_airport, f.destination, f.destination_airport,
    f.seats, f.return_flight_booked,
    f.departure_time, f.return_time,
    f.checked_bags, f.trip_airline,
    f.destination_airport_lat, f.destination_airport_lon,
    f.base_fare_usd,
    -- Hotel details
    h.hotel_name, h.nights, h.rooms,
    h.check_in_time, h.check_out_time,
    h.hotel_per_room_usd AS hotel_price_per_room_night_usd

  FROM sessions_2023 s
  INNER JOIN users_spark u       ON s.user_id = u.user_id   -- Must have a user record
  LEFT JOIN  flights_spark f     ON s.trip_id = f.trip_id   -- Optional flight data
  LEFT JOIN  hotels_spark h      ON s.trip_id = h.trip_id   -- Optional hotel data

  WHERE s.user_id IN (SELECT user_id FROM filtered_users)   -- Apply the >7 session filter
)

SELECT * FROM session_base;
```

### Verify the Row Count

After running the above, confirm the record count:

```sql
-- Should return approximately 49,000
SELECT COUNT(*) FROM workspace.default.sessions_filtered;
```

### Why These Filters?

| Filter | Reason |
|---|---|
| `session_start > '2023-01-04'` | Focuses on the post-Winter-Sale cohort — users acquired during the campaign spike |
| `HAVING COUNT(*) > 7` | Removes users with minimal platform engagement, ensuring the RFM model has enough signal per user |
| `INNER JOIN users_spark` | Ensures every session row has a valid user profile — no orphaned session records |

### Export the Filtered Records to CSV (from Databricks)

```python
# Load the filtered table into Pandas
df_filtered = spark.table("workspace.default.sessions_filtered").toPandas()

print(f"Total records: {len(df_filtered):,}")          # Should be ~49,000
print(f"Unique users: {df_filtered['user_id'].nunique():,}")   # Should be ~5,998

# Export to DBFS for download
df_filtered.to_csv(
    '/dbfs/FileStore/sessions_filtered_49k.csv',
    index=False
)
# Then download from: Databricks UI → Data → DBFS → FileStore
```


## 8. CSV File — User Perk Assignments

This CSV contains one row per user with their aggregated behavioural features and their assigned rewards perk.

### Column Reference

| Column | Type | Description |
|---|---|---|
| `user_id` | string | Unique user identifier |
| `session_created` | int | Total number of sessions created by the user |
| `total_flights_booked_not_cancelled` | int | Count of confirmed (non-cancelled) flight bookings |
| `total_hotels_booked_not_cancelled` | int | Count of confirmed hotel bookings |
| `total_trip_cancellation` | int | Total cancellations |
| `avg_flight_base_fare_booked` | float | Average base fare across confirmed flight bookings |
| `avg_hotel_price_per_room_booked` | float | Average nightly hotel rate across confirmed hotel bookings |
| `avg_nights_booked` | float | Average hotel stay duration |
| `gender` | string | User's gender |
| `marital_status` | boolean | Whether user is married |
| `children` | boolean | Whether user has children |
| `home_country` | string | User's home country |
| `home_airport` | string | User's primary departure airport (IATA code) |
| `rfm_recency_days` | int | Days since sign-up (Recency score input) |
| `rfm_frequency` | int | Session count (Frequency score input) |
| `rfm_monetary` | float | Combined avg fares (Monetary score input) |
| `rfm_score` | float | Composite RFM score |
| `rfm_segment` | string | RFM segment label: New / Engaged / Frequent / Other |
| `assigned_perk` | string | **The single perk assigned to this user** |

### Perk Assignment Rules (Priority Order)

```python
def assign_perk(row):
    if flights >= 2 and avg_flight_discount > 50:   return "Flight Discount"
    if hotels >= 2  and avg_hotel_discount > 50:    return "Hotel Discount"
    if children == True and avg_nights >= 3:         return "Family Perk"
    if sessions >= 10:                               return "Loyalty Perk"
    if sign_up_date >= 2026-01-01:                  return "New User Perk"
    return "Other"
```

### Sample Data Preview

```
user_id    | sessions | flights | hotels | avg_fare | avg_hotel | rfm_segment | assigned_perk
-----------|----------|---------|--------|----------|-----------|-------------|---------------
u_100001   | 12       | 3       | 2      | 342.50   | 145.00    | Engaged     | Loyalty Perk
u_100002   | 4        | 0       | 0      | 0.00     | 0.00      | Other       | Other
u_100003   | 8        | 2       | 3      | 275.00   | 189.00    | Engaged     | Flight Discount
u_100004   | 15       | 1       | 4      | 195.00   | 220.00    | Engaged     | Loyalty Perk
u_100005   | 6        | 3       | 2      | 520.00   | 95.00     | Engaged     | Flight Discount
```

## 9. Dependencies

### Python Packages

```txt
# requirements.txt
pyspark>=3.4.0
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
seaborn>=0.12.0
scipy>=1.10.0
squarify>=0.4.3
```

### Install All Dependencies

```bash
pip install -r requirements.txt
```

### Install Individually

```bash
pip install pyspark pandas numpy matplotlib seaborn scipy squarify
```

### Inside a Databricks Notebook Cell

```python
# Install squarify (the only package not pre-installed on Databricks runtime)
%pip install squarify

# All other packages (pandas, numpy, matplotlib, seaborn, scipy) are
# pre-installed on Databricks Runtime 13.3 LTS and above.
```

### Platform Requirements

| Requirement | Version |
|---|---|
| Python | 3.10 or higher |
| Apache Spark | 3.4 or higher |
| Databricks Runtime | 13.3 LTS or higher (recommended) |
| Java | 11 or higher (required by Spark) |


## 10. Visualizations

All charts were created in a notebook using **Matplotlib** and **Seaborn**. The following visualizations are included in the analysis:

### Session-Level EDA (5 Charts)
| # | Chart | Business Question |
|---|---|---|
| 1 | Daily Active Users over Time | Is platform engagement growing or declining? |
| 2 | Engagement Tier vs Conversion Rate | Do highly engaged users actually convert? |
| 3 | Session Outcome Distribution (pie) | What % of sessions result in a bundle vs flight-only vs browse-only? |
| 4 | Booked vs Cancelled by Search Depth | Are cancellations driven by low-intent or high-intent sessions? |
| 5 | Sessions, Bookings & Revenue by Hour | When during the day do users actually spend money? |

### User-Level EDA (5 Charts)
| # | Chart | Business Question |
|---|---|---|
| 1 | User Sign-Up Trends over Time | When did we acquire most users and why? |
| 2 | January 2023 Cohort Daily Sign-Ups | What drove the spike and how do we leverage it? |
| 3 | Family & Marital Structure breakdown | What is our user's household profile? |
| 4 | Top Home Airports by User Count | Where should we focus geo-targeted campaigns? |
| 5 | Age-Concentrated Distribution | Are we missing key demographics? |

### Segmentation Charts
- **RFM Treemap** — User count by segment label (Engaged / Frequent / Other)
- **Perk Allocation Treemap** — User count by assigned perk
- **A/B Test Retention Bar Chart** — Control vs Treatment by perk type
- **Monthly Retention Cohort Lines** — Retention tracking over time by perk and variant



| ✅ Usage Instructions | Done | Section 6 |
| ✅ Directory Structure Explanation | Done | Section 4 |
| ✅ CSV with users assigned to perks | Done | [`data/traveltide_user_perk_assignments.csv`](data/traveltide_user_perk_assignments.csv) |
| ✅ How to extract the ~49,000 filtered records | Done | Section 7 (full SQL + export code) |
| ✅ Dependencies and how to install them | Done | Section 9 |
