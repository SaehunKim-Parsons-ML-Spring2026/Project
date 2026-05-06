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
 
## Milestone 04: Final Classification Model

### Project Focus

For Milestone 04, I built the final classification experiment for the outdoor dining project.

The goal was to test whether outdoor dining locations that remained active could be distinguished from locations that disappeared.

This model does not explain why each restaurant stayed or disappeared. Instead, it tests whether the two groups are partly classifiable using measurable local and spatial features.

### Research Question

Can a Random Forest classifier distinguish remained outdoor dining locations from disappeared outdoor dining locations?

### Target Variable

The target variable is `survived`.

`survived = 1` means the location is still active in the current Dining Out NYC dataset.

`survived = 0` means the location appeared in the historic emergency outdoor dining dataset but is no longer active.

### Final Dataset

The final dataset contains Manhattan outdoor dining locations from both the historic emergency program and the current Dining Out NYC program.

Historic locations: 4,659

Current locations: 318

Total locations: 4,977

Class imbalance: approximately 1:14

### Features Used

Local context features:

`dist_subway_m`

`is_near_transit`

`streetwidth`

`streetwidth_missing`

Spatial repetition features:

`is_corridor`

`same_street_nearby_count_1000ft`

`nearby_outdoor_dining_count_300ft`

### Feature Engineering

I created a local same street repetition feature called `same_street_nearby_count_1000ft`.

This counts nearby outdoor dining locations within 1000 feet that share the same street name.

I used this instead of a simple street count because long avenues can stretch across Manhattan. This keeps the feature local instead of treating an entire avenue as one pattern.

I also used `nearby_outdoor_dining_count_300ft` to measure nearby outdoor dining density.

These features are not causal. They test whether nearby spatial repetition helps classify remained versus disappeared locations.

### Models Tested

I tested four models:

Dummy baseline

Random Forest using local context features

Random Forest using spatial repetition features

Random Forest using combined features

The dummy baseline was used because the dataset is highly imbalanced. It shows why accuracy alone is not enough for this project.

### Final Result

The combined Random Forest model performed best.

Accuracy: 0.807

Balanced accuracy: 0.722

Precision for survived class: 0.192

Recall for survived class: 0.625

F1 score for survived class: 0.294

The model found about 63% of the remained locations in the test set, but it also made many false positive predictions.

### Interpretation

The strongest signal came from `same_street_nearby_count_1000ft`.

This means nearby repetition on the same street helped the model separate remained and disappeared locations.

This does not mean that same street repetition caused outdoor dining to remain.

The result is limited but useful: remained locations are partly distinguishable through local spatial repetition.

### Limitations

This is an exploratory classification model, not a causal model.

The model does not include many real factors that may affect outdoor dining decisions, such as permit cost, storage, seasonal decisions, enforcement, restaurant revenue, ownership decisions, or exact sidewalk setup constraints.

The 1000 foot threshold is a design choice. A future version could test multiple distances, such as 500 feet, 1000 feet, and 1500 feet.

Street width was only matched for about 52.5% of locations, so I used imputation and a missingness indicator.

The model also produced many false positives, so it should not be used to predict individual restaurant outcomes.

### Conclusion

The final model does not explain why specific restaurants stayed.

But it shows that remained and disappeared locations are partly distinguishable.

The ML result supports the larger project finding: outdoor dining did not remain evenly across Manhattan. What remains has a measurable street level pattern.
