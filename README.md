# RIPTA Transit Reliability & Ridership Analysis

This repository contains a full data pipeline for integrating RIPTA AVL data, GTFS schedules, and ridership data to analyze transit reliability and passenger demand.

The pipeline is implemented in a single R Markdown file and produces clean, structured outputs for further analysis and visualization.

---

## Project Overview

This project aims to answer:

- How reliable is the transit system (On-Time Performance, OTP)?
- How does reliability relate to ridership?
- Which routes and trips perform best or worst?

To achieve this, we build a multi-step pipeline that:

1. Maps AVL stops to GTFS stops  
2. Matches AVL trips to GTFS trips (daily level)  
3. Builds stable trip relationships (monthly level)  
4. Calculates reliability metrics (OTP, weighted OTP)  
5. Produces route-level and trip-level summaries  

---

## Pipeline Structure

The entire workflow is implemented in:

```

RIPTA_Merge_Data_daily_monthly.rmd

```

### Step 1 — Stop Mapping (AVL → GTFS)

- Match AVL coordinates to GTFS stops using spatial nearest neighbor
- Assign:
  - `Mapped_StopId`
  - `mapping_flag` (good / ok / far / missing)

Only high-quality mappings are used in later steps.

---

### Step 2 — Daily Trip Matching

- Match:
  - `AppID` (GTFS trip)
  - `SystemID` (AVL trip)

Matching is based on:

- Time overlap (IoU)
- Stop similarity
- Temporal proximity

Output:

```

daily_match_*.csv

```

---

### Step 2.2 — Monthly 1-to-1 Trip Bridge

- Identify stable relationships between:
  - AppID ↔ SystemID

Across multiple days:

- Consistency
- Dominant mapping share

Output:

```

monthly_bridge_*.csv

```

---

### Step 3 — Reliability (OTP) Analysis

#### Trip-level OTP

- Based on AVL deviation:
  
```

on_time_point = abs(Deviation) <= threshold

```

- Aggregate to trip-level:

```

trip_otp = mean(on_time_point)
trip_on_time = trip_otp >= 0.5

````

---

#### Route-level Metrics

For each route:

- OTP (unweighted)
- Weighted OTP (by ridership)
- Mean trip OTP

Key definition:

> **Weighted OTP = ridership-weighted average of trip-level on-time results**

---

#### Trip-level Rankings

Identify:

- Trips most frequently late
- Trips with highest ridership
- Trips with worst reliability

---

## Outputs

The pipeline generates a small set of clean outputs:

### Core Tables

- `mapped_avl_*.csv`  
- `daily_match_*.csv`  
- `monthly_bridge_*.csv`  
- `route_summary_*.csv`  
- `trip_rankings_*.csv`  

---

### Visualization

- OTP vs Weighted OTP scatter plot

---

## Key Concepts

### Trip Matching

- Aligns operational trips (AVL) with scheduled trips (GTFS)
- Enables joint analysis of:
  - reliability
  - ridership

---

### Weighted OTP

- Reflects **actual passenger experience**
- High-ridership trips contribute more

---

### Data Filtering

We exclude:

- `AppID = 0` (invalid trips)
- Poor stop mappings (`far`, `missing_coord`)
- NA coordinates

---

## How to Run

### 1. Update File Paths

Modify at the top of the Rmd:

```r
ridership_file <- "..."
system_file <- "..."
stops_file <- "..."
````

---

### 2. Set GTFS Version

```r
gtfs_tag <- "gtfs20240112"
```

This controls:

* input GTFS file
* output folder naming

---

### 3. Key Parameters

You can tune:

```r
OTP_THRESHOLD_MIN <- 5        # on-time threshold (minutes)
MIN_DAYS_FOR_RANKING <- 3     # minimum observations for trip ranking
KEEP_MAPPING_FLAGS <- c("good", "ok")
```

---

### 4. Run the Pipeline

Open the Rmd and run all chunks:

```
Knit → HTML
```

or

```
Run All
```

---

## Notes & Design Choices

* Pipeline is **modular but linear**
* Intermediate objects are removed to save memory
* Output is intentionally minimal and clean

---

## Limitations

* Matching is heuristic-based (IoU, stops, time)
* Some routes have small sample size (e.g., Route 10, 12)
* GTFS changes across seasons → must update version parameter

---

## Future Work

* Driver transition / route switching analysis
* Graph-based modeling (trip network)
* Clustering of trip patterns
* Interactive dashboard (Shiny)

---

## Author

RIPTA Transit Data Analysis Project
Brown University collaboration
Under guidance of Prof. Alice Paul


