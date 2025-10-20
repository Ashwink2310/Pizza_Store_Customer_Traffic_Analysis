# Pizza Store Customer Traffic Analysis

## TLDR
Built a machine learning pipeline to **predict customer occupancy** at a pizza store and **cluster daily demand patterns** using order history, weather data, and Google Trends. Achieved **98.8 percent accuracy** for peak group sizes using a **Voting Classifier**, and identified **three demand clusters** to guide **staffing, inventory, and targeted promotions**.

---

## Objective
Help a pizza shop make **better staffing and promotion decisions** by:

- Predicting **how busy the store will be at any given time**
- Grouping **similar days into demand clusters**
- Providing **actionable business recommendations**

---

## Data Sources

| Data Type      | Details |
|----------------|---------|
| Orders & Line Items | Pizza orders with timestamp, size, quantity, price |
| Pizza Metadata | Category, ingredients |
| Weather Data | Temperature, precipitation, events |
| Google Trends | Public interest in “pizza” by date |

---

## Feature Engineering Highlights

- Extracted **day of week, season, hour** from timestamps  
- Computed **per-order and per-day revenue**  
- Modeled **seating duration with a 45 minute stay window**  
- Simulated **per 5 minute occupancy** to estimate presence levels  
- Labeled traffic into **Low / Medium / High** using **dynamic thresholds by group size**

---

## Modeling Approach

### **Classification (Occupancy Level Prediction)**
| Model | Purpose | Outcome |
|--------|---------|---------|
| Decision Tree | Baseline | Simple but overfit |
| Random Forest | Improve robustness | Better generalization |
| **Voting Classifier (Final)** | Soft voting of Logistic + Tree + RF | ✅ **98.8 percent accuracy for 4+ group size** |

---

### **Clustering (Daily Demand Segmentation)**

| Method | Result | Outcome |
|--------|--------|---------|
| KMeans | Overlapping clusters | ❌ Not ideal |
| **Agglomerative Clustering (Final)** | 3 distinct groups | ✅ Clear interpretability |

---

## Cluster Insights

| Cluster | Behavior | Recommendation |
|---------|----------|----------------|
| Cluster 0 | High revenue from **Chicken & Classic large pizzas**, strong on **Fridays** | Push **weekend deals** |
| Cluster 1 | **Veggie & Supreme** lovers, strong on **Sundays** | Promote **Veggie variants** |
| Cluster 2 | Balanced sales across **Tue/Thu** | Run **midweek bundles** |

---

## Key Recommendations

- Promote **Large Chicken & Classic** pizzas across clusters  
- Launch **Veggie-heavy offers** for Cluster 1  
- **Weekend promos** for Clusters 0 & 1  
- **Midweek specials** for Cluster 2  

---

## 📁 Repository Structure

