# RIPTA_Research_Spring26
Data pipeline for matching RIPTA AVL data with GTFS trips to analyze transit reliability, ridership, and trip-level performance.

# RIPTA Transit Reliability Analysis

This repository contains code for integrating RIPTA AVL data, GTFS schedules, and ridership data to analyze transit reliability and passenger demand.

The goal of this project is to build a consistent pipeline to match AVL trips with GTFS trips and evaluate on-time performance (OTP) at both route and trip levels.

---

## Project Goals

- Match AVL operational data with GTFS scheduled trips
- Link matched trips with ridership information
- Calculate reliability metrics such as On-Time Performance (OTP)
- Analyze reliability at both route and trip levels
- Explore relationships between reliability and passenger demand

---

## Data Sources

This project integrates three main datasets:

1. **AVL Data**
   - Automatic Vehicle Location records
   - Includes vehicle timestamps and schedule deviation

2. **GTFS Data**
   - Scheduled trip and stop information
   - Used as the reference schedule

3. **Ridership Data**
   - Passenger counts associated with GTFS trips

---

## Analysis Pipeline

The project builds a multi-step data processing pipeline:

### 1. Stop Mapping
- Map AVL stops to GTFS stops
- Keep only high-confidence mappings

### 2. Daily Trip Matching
- Match AVL trips (`SystemID`) to GTFS trips (`AppID`)
- Perform trip matching at the daily level

### 3. Monthly 1-to-1 Trip Bridge
- Identify stable trip relationships across multiple days
- Create a monthly bridge between AVL and GTFS trips

### 4. Reliability Calculation
Compute reliability metrics using AVL deviation data:

- On-Time Performance (OTP)
- Weighted OTP (ridership-weighted reliability)
- Trip-level reliability metrics

### 5. Analysis Outputs
Generate summary outputs including:

- Route-level OTP statistics
- OTP vs Weighted OTP comparison
- Trip-level reliability rankings
- Identification of trips with high ridership or frequent delays

---

## Example Outputs

The analysis produces:

- Route-level OTP summary tables
- Scatter plots comparing OTP vs weighted OTP
- Trip-level ranking tables:
  - Most frequently late trips
  - Trips with highest ridership
  - Trips with worst reliability

---

## Future Work

Possible next steps include:

- Driver transition analysis across routes
- Graph-based modeling of trip connections
- Clustering of trip transition networks
- Interactive dashboards (e.g., Shiny app)

---


## Notes

- Large datasets (AVL / GTFS raw files) are not included in this repository.
- The code assumes access to the RIPTA data environment.

