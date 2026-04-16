# Project
Project: Milestone 01

---

## Option 1: Street Corridor Typology

### Idea
This project explores how outdoor dining locations cluster along specific types of streets.

### Data
- Dining Out NYC Locations  
- NYC Street Network (LION)  

### Method
- Spatial join (points to street segments)  
- Clustering (K-means or DBSCAN)

---

## Option 2: Complaint Patterns Around Outdoor Dining

### Idea
This project investigates how public complaints reflect different types of friction in outdoor dining environments.

### Data
- NYC 311 complaints  
- Dining locations  

### Method
- Filtering outdoor dining complaints  
- Clustering (K-means or hierarchical clustering)

---

## Option 3: Transit Accessibility and Outdoor Dining

### Idea
This project predicts which locations in New York City are likely to support outdoor dining based on spatial conditions such as transit accessibility, complaint intensity, and surrounding infrastructure.

### Data
- Outdoor dining locations (historic + current)
- Subway stations
- 311 complaints

### Method
- Feature engineering (distance to transit, complaint density)
- Binary classification (survival vs non-survival)
- Logistic regression / Random Forest
- Feature importance analysis

### Goal
To identify which spatial factors most strongly influence whether outdoor dining can persist.

---

## Data Sources

- Dining Out NYC Locations (NYC Open Data)

  https://data.cityofnewyork.us/Transportation/Dining-Out-NYC-Locations/fpeh-f7ci  

- NYC 311 Service Requests (2020–Present)

  https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2020-to-Present/erm2-nwe9  

- NYC Street Network

  https://www.nyc.gov/content/planning/pages/resources/datasets/lion  

- MTA Subway Stations

  https://data.ny.gov/Transportation/MTA-General-Transit-Feed-Specification-GTFS-Static/fgm6-ccue

- Dining Out NYC Program (rules, fees, and guidelines)

  https://www.diningoutnyc.info/  


## Milestone 02: Data Preparation & Exploration

### Project Focus

From the initial three directions, the project proceeds with:

👉 **Option 3: Transit Accessibility and Outdoor Dining**

The focus shifts from exploring multiple ideas to a single question:

> What spatial conditions are associated with where outdoor dining remains?

This stage does not aim to build a predictive model yet.  
Instead, it prepares and evaluates the data to determine whether meaningful patterns exist.

---

## Dataset Construction

The dataset combines historic and current outdoor dining locations in Manhattan.

### Sources

- Historic outdoor dining applications (NYC Open Data)  
- Current licensed locations (Dining Out NYC)  
- MTA subway stations  
- NYC street network (LION)  
- NYC 311 complaints (exploratory use only)

---

### Dataset Summary

| | Count |
|---|---|
| Historic locations (survived=0) | 4,659 |
| Current locations (survived=1) | 318 |
| Total | 4,977 |
| Class imbalance | 1:14 |

---

### Features

| Feature | Type | Description |
|--------|------|-------------|
| `dist_subway_m` | continuous | Distance to nearest subway station |
| `is_near_transit` | binary | Within 400m of subway |
| `is_corridor` | binary | Located on key corridor streets |
| `streetwidth` | continuous | Proxy for physical street capacity |

---

### Target

- `survived` (binary)  
  - 1: currently licensed  
  - 0: historic (no longer active)

---

## Data Processing

- Filtered to Manhattan only  
- Removed duplicate locations (coordinate-based)  
- Normalized street names across datasets  
- Computed nearest subway distance using spatial index (KDTree)  
- Created binary features (transit access, corridor membership)  
- Matched street width using street network data  

---

## Exploratory Analysis

Exploratory analysis was conducted to evaluate differences between surviving and non-surviving locations.

### Key observations

- **Transit access**
  - Surviving locations tend to be closer to subway stations  

- **Street corridors**
  - Outdoor dining clusters along a small number of streets  

- **Street width**
  - Wider streets are more frequently associated with surviving locations  

These patterns suggest that survival is associated with a combination of:

- access  
- street structure  
- physical capacity  

---

## PCA (Feature Space Exploration)

Principal Component Analysis was used to explore the structure of the feature space.

- PC1 is dominated by transit-related features  
- PC2 reflects corridor membership  

This indicates that different spatial factors contribute along different dimensions.

---

## Limitations

- **Class imbalance (1:14)**  
  Far more historic than current locations  

- **Complaint data unavailable for historic dataset**  
  Used only in exploratory analysis  

- **Street width coverage (~52%)**  
  Due to spatial matching between point data and street segments  

- **Correlation ≠ causation**  
  Findings are associative, not causal  

---

## Next Steps (Milestone 03)

- Train classification models:
  - Logistic Regression  
  - Random Forest  