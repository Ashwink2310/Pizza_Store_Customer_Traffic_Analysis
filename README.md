# Pizza Store Customer Traffic Analysis

A comprehensive machine learning system for restaurant demand forecasting, operational clustering, and business optimization using time-series sales data, weather integration, and trend analysis.

## Project Overview

This project develops an end-to-end analytics solution for a pizza restaurant to predict customer demand, identify business patterns, and optimize staffing through data-driven insights. The system combines multi-output regression, unsupervised clustering, and cost modeling to transform transactional data into actionable operational intelligence.

## Key Highlights

- **Dataset**: 49,574 pizza orders across 358 operational days (2015)
- **Multi-Output Regression**: Simultaneous forecasting of 5 customer seating categories
- **Performance**: 35-52% improvement over baseline (MAE 0.44, R² 0.50-0.60)
- **Clustering**: K-Means and Hierarchical analysis identifying 2-3 distinct operational day types
- **Business Impact**: Staffing cost model quantifying labor optimization and revenue protection

## Dataset

The project uses pizza restaurant transactional data with external enrichment:

### Core Data
- **Transactions**: 49,574 individual pizza orders
- **Orders**: 21,350 unique customer orders
- **Period**: January 1 - December 31, 2015
- **Pizza Varieties**: 32 types across 4 categories (Classic, Chicken, Supreme, Veggie)
- **Sizes**: S, M, L, XL, XXL

### External Data
- **Weather**: NYC 2015 daily weather (temperature, precipitation, wind, events)
- **Trends**: Google search volume for pizza-related terms (weekly by category)

**Note**: Dataset files are not included in this repository. Sample data format provided in `/data/README.md`.

### Key Features
- **Temporal**: Hour, minute, day of week, day of month, month, season
- **Operational**: Pizza type, size, category, ingredients (79 binary columns)
- **External**: Weather conditions, temperature, precipitation, trend scores
- **Derived**: Rolling averages, time interactions, aggregated daily patterns

## Methodology

### 1. Data Integration & Feature Engineering
- Merge transactional data (orders, pizzas, types)
- Integrate NYC weather data via date matching
- Engineer 79 ingredient binary features from text parsing
- Extract temporal features at multiple granularities
- Create seating intervals (5-minute bins with 45-minute occupancy assumption)

### 2. Multi-Output Demand Forecasting
- **Target**: Predict customer counts across 5 seating categories simultaneously
  - Seated_1: Individual diners
  - Seated_2: Couples
  - Seated_3: Groups of 3
  - Seated_4: Groups of 4
  - Seated_4+: Large parties (5+ people)
- **Model**: Random Forest Regressor (100 trees, depth 15, min_split 5)
- **Preprocessing**: One-hot encoding (categorical), standard scaling (numerical)
- **Performance**: 
  - Seated_1: MAE 0.65 (40% improvement), R² 0.60
  - Seated_2: MAE 0.58 (34% improvement), R² 0.55
  - Seated_3: MAE 0.42 (36% improvement), R² 0.50
  - Seated_4: MAE 0.43 (35% improvement), R² 0.50
  - Seated_4+: MAE 0.12 (52% improvement), R² 0.59
  - **Overall**: MAE 0.44, RMSE 0.63

### 3. Clustering for Operational Segmentation
- **Data**: Daily aggregates (358 days × 29 features)
  - Pizza counts by size (S, M, L, XL, XXL)
  - Category distribution (Chicken, Classic, Supreme, Veggie)
  - Category-size combinations
  - Revenue, weather conditions
- **Dimensionality Reduction**: PCA (29 features → 11 components)
- **K-Means Clustering**: 
  - Optimal k=3 (silhouette score maximization)
  - Identifies high-volume, normal, and low-volume day types
- **Hierarchical Clustering**:
  - Optimal k=2 (Ward linkage, silhouette scoring)
  - 83.6% typical days, 16.4% exceptional days

### 4. Business Impact Modeling
- **Staffing Costs**: $15/hour per staff, 1 staff per 10 customers, 8-hour shifts
- **Opportunity Costs**: $15 revenue per customer, 10% loss rate when understaffed
- **Metrics**: Daily customer range 77-266 (mean 138 ± 24.5)
- **Analysis**: Compare ML-informed dynamic staffing vs. static baseline

## Key Results

### Model Performance
| Target | Baseline MAE | Model MAE | RMSE | R² | Improvement |
|--------|-------------|-----------|------|-----|-------------|
| Seated_1 | 1.07 | **0.65** | 0.84 | 0.60 | 39.6% |
| Seated_2 | 0.88 | **0.58** | 0.77 | 0.55 | 34.0% |
| Seated_3 | 0.66 | **0.42** | 0.56 | 0.50 | 35.9% |
| Seated_4 | 0.66 | **0.43** | 0.56 | 0.50 | 35.2% |
| Seated_4+ | 0.24 | **0.12** | 0.26 | 0.59 | 51.7% |
| **Overall** | - | **0.44** | **0.63** | - | - |

### Operational Insights
- **Peak Hours**: 12-1 PM (lunch), 5-7 PM (dinner)
- **Busiest Day**: Friday (8,242 pizzas), Slowest: Sunday (6,035 pizzas)
- **Weather Impact**: 70% orders on sunny days, 30% reduction in rain/snow
- **Size Preference**: Large (38%), Medium (35%), Small (19%)
- **Category Leaders**: Classic (33%), Supreme (27%), Veggie (24%), Chicken (16%)

### Clustering Results
- **K-Means (k=3)**: Three distinct operational modes
- **Hierarchical (k=2)**: 83.6% typical vs 16.4% exceptional days
- **Silhouette Scores**: 0.28-0.32 (K-Means), 0.35-0.40 (Hierarchical)

## Business Applications

1. **Staffing Optimization**: Predict customer demand 5-minute intervals → dynamic shift scheduling
2. **Inventory Management**: Pizza size/category forecasts → targeted ingredient purchasing
3. **Reservation Strategy**: Large party predictions → capacity management decisions
4. **Marketing Campaigns**: Cluster-based targeting (promotions on predicted slow days)
5. **Service Quality**: Right-sized staffing → reduced wait times and customer satisfaction

## Challenges & Limitations

1. **Data Quality**: Missing weather data for some dates required imputation
2. **Class Imbalance**: Hierarchical clustering 83%/17% split raises generalizability concerns
3. **Limited Event Features**: Holidays, local events, sports games not captured
4. **Occupancy Assumption**: Fixed 45-minute duration oversimplifies varying dining times
5. **5-Minute Resolution**: High granularity may introduce noise from random arrival timing

## Future Improvements

- [ ] Integrate holiday calendar and local event data
- [ ] Real-time weather API integration for dynamic forecast updates
- [ ] Reservation system data for forward-looking booking features
- [ ] Gradient boosting ensembles (XGBoost, LightGBM) for accuracy gains
- [ ] LSTM/Transformer models for explicit temporal dependency learning
- [ ] Bayesian approaches for uncertainty quantification
- [ ] Production API deployment with monitoring dashboards
- [ ] A/B testing ML-informed vs intuition-based staffing
- [ ] POS system integration for automated data ingestion
