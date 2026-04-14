# Santander Cycles Station Segmentation and Atypical Station Detection

## Project Overview

This repository contains two connected unsupervised learning projects built on Santander Cycles station behaviour data.

The main goal is to understand how docking stations differ in operational usage patterns so that station monitoring, bike rebalancing, and capacity planning can be better informed.

The project is presented in two parts:

- **K-Means station segmentation** for broad operational grouping
- **DBSCAN analysis** for density-based comparison and atypical station detection

Together, these notebooks show not only how clustering can be used to group similar stations, but also how different clustering methods can lead to different practical insights.

---

## Business Problem

In a bike-sharing system, stations do not all behave in the same way.

Some stations experience strong morning departures, some receive more bikes than they send, some are more active during weekends, and others show unusual imbalance or timing patterns. These behavioural differences can create practical operational issues such as:

- stations running out of bikes
- stations running out of empty docks
- uneven network flow
- stations requiring different rebalancing attention

Because of this, it is useful to identify both:

- the **main station behaviour types** across the network
- the **stations that do not fit the dominant pattern**

---

## Project Objectives

This repository aims to:

- build station-level behavioural profiles from Santander Cycles journey data
- cluster stations based on operational usage characteristics
- compare a centroid-based method (**K-Means**) with a density-based method (**DBSCAN**)
- assess which method is more useful for the business problem
- identify both standard station groups and atypical stations

---

## Dataset

The main K-Means notebook uses official Santander Cycles journey data from Transport for London (TfL), using a recent **August 2025** subset to keep the workflow manageable while still reflecting realistic station behaviour.

The raw journey data includes fields such as:

- start station
- end station
- timestamps
- bike type
- trip duration

From the raw journey records, a cleaned **station-level feature table** was created where each row represents one station.

The DBSCAN notebook uses this engineered station-level dataset directly:

- `station_features_cleaned.csv`

---

## Feature Engineering Summary

The raw trip data was transformed into station-level behavioural features designed to capture demand intensity, timing structure, and operational imbalance.

Example feature groups include:

### Activity Features
- total departures
- total arrivals
- average daily departures
- average daily arrivals
- active days

### Timing Features
- weekday departures
- weekend departures
- morning peak departures
- evening peak departures
- weekend departure share
- morning peak departure share
- evening peak departure share

### Operational Features
- average departure duration
- net flow
- imbalance ratio

These features allow stations to be compared using behaviour rather than just location or identity.

---

## Repository Files

- `santander_station_segmentation_kmeans.ipynb`  
  Main station segmentation notebook using K-Means

- `santander_station_segmentation_dbscan.ipynb`  
  Density-based clustering notebook using DBSCAN

- `station_features_cleaned.csv`  
  Cleaned station-level feature dataset used in the DBSCAN analysis

---

## Notebook 1: K-Means Station Segmentation

This notebook is the main clustering project in the repository.

### Purpose
To segment Santander Cycles stations into operationally meaningful behaviour groups using K-Means clustering.

### What the notebook covers
- loading and combining recent journey data
- cleaning and validating trip records
- feature engineering at station level
- scaling and clustering
- selecting the final number of clusters
- cluster profiling and interpretation
- operational recommendations by group

### Main outcome
The final K-Means solution used **K = 5** and identified five distinct station behaviour groups.

These included patterns such as:

- longer-duration, weekend-leaning stations
- high-volume central active stations
- standard everyday commuter-style stations
- special operational or anomalous stations
- evening-oriented central destination stations

### Why it matters
K-Means worked well as the primary segmentation method because it produced broad, interpretable station groups that could support:

- station profiling
- rebalancing prioritisation
- demand pattern understanding
- operational planning

---

## Notebook 2: DBSCAN Analysis for Atypical Station Detection

This notebook is a supporting extension of the main project.

### Purpose
To test whether DBSCAN, a density-based clustering method, can reveal meaningful operational structure beyond the K-Means baseline.

### What the notebook covers
- loading the cleaned station-level feature table
- removing identifiers and previous outputs from model input
- checking for missing values
- scaling behavioural features
- fitting DBSCAN with multiple parameter settings
- evaluating the effect of changing `eps` and `min_samples`
- inspecting noise-labelled stations

### Main outcome
DBSCAN did **not** recover several large, meaningful density-based station groups in this feature space.

Instead, it mostly identified:

- one broad dense group
- many noise-labelled stations
- only a few very small additional groups

### Practical interpretation
This means DBSCAN was **not the strongest method for primary station segmentation** in this dataset.

However, it still added useful value by identifying stations that sit outside the dominant dense behavioural pattern. Those stations are better viewed as **candidates for further operational review** rather than as errors.

---

## Key Comparison: K-Means vs DBSCAN

Two clustering approaches were applied to the same station-level behavioural dataset.

### K-Means
K-Means produced interpretable station groups and was more effective for broad operational segmentation across the network.

It worked well for identifying common station types and translating them into operationally useful categories.

### DBSCAN
DBSCAN was tested as a density-based alternative.

In this dataset, it mainly identified one broad dense group together with many noise-labelled stations. This suggests DBSCAN was less suitable here for full segmentation, but still useful for identifying stations outside the dominant behavioural pattern.

### Practical takeaway
For this station feature space:

- **K-Means** is the stronger method for primary segmentation
- **DBSCAN** is better used as a supporting method for atypical station detection

This comparison strengthens the project because it shows that model choice should be driven by data structure and business usefulness, not just by trying more algorithms.

---

## Main Findings

- Santander Cycles stations do **not** behave uniformly
- Station behaviour differs across demand level, timing structure, duration profile, and imbalance
- K-Means produced clearer and more interpretable operational groups
- DBSCAN mostly found one dominant dense population plus atypical stations outside that pattern
- The two methods provide complementary insight:
  - K-Means for broad segmentation
  - DBSCAN for identifying unusual stations

---

## Business Value

This project is relevant to operational planning because station-level segmentation can help support:

- targeted rebalancing strategies
- identification of high-risk imbalance stations
- monitoring of non-standard station behaviour
- prioritisation of operational attention across the network

The project also shows a realistic analytical workflow:

- move from raw journey data to engineered station-level features
- apply clustering methods
- interpret results in practical operational terms
- compare model suitability rather than forcing one method to “win”

---

## How to Run

1. Clone or download this repository
2. Open the notebooks in Jupyter Notebook or JupyterLab
3. Make sure the required Python libraries are installed
4. Run the cells in order

### Main Python libraries used
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn

---

## Limitations

### K-Means limitations
- K-Means assumes relatively compact cluster structure
- The final grouping depends on the selected feature set
- The analysis is based on one recent monthly period, so it may not fully capture longer-term seasonal behaviour

### DBSCAN limitations
- DBSCAN did not recover several large, well-separated station groups in this feature space
- The results were sensitive to `eps` and `min_samples`
- The method was more useful for atypical station review than full segmentation in this project

### General limitations
- The analysis is based on behavioural station summaries rather than live dock availability or real-time rebalancing events
- External factors such as weather, events, station capacity, and location metadata were not incorporated into the current modelling workflow

---

## Future Improvements

Possible next steps include:

- extending the analysis across multiple months
- adding station capacity and location metadata
- incorporating weather or event data
- profiling DBSCAN noise stations in more depth
- comparing with other clustering methods such as Agglomerative Clustering
- building a dashboard to explore station groups interactively

---

## Final Summary

This repository presents a complete unsupervised learning case study built around a real transport operations problem.

The project shows that:

- clustering can be used to build meaningful station behaviour profiles
- K-Means is more effective here for broad station segmentation
- DBSCAN adds value by surfacing stations outside the dominant behavioural pattern
- comparing models honestly leads to stronger analytical conclusions than treating every method as a success

This makes the repository a stronger portfolio piece than a single notebook, because it demonstrates:

- feature engineering
- clustering application
- model comparison
- business interpretation
- practical analytical judgment
