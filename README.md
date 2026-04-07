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
TThis project predicts which locations in New York City are likely to support outdoor dining based on spatial conditions such as transit accessibility, complaint intensity, and surrounding infrastructure.

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
