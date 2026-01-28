# Water Quality Classification & Public Health Analytics

## Problem Statement
Access to safe water is a critical public health concern. This project analyzes water quality data from Maharashtra, India, and builds machine learning models to classify water bodies into designated use-based quality classes — **Class A, B, C, and E** — using physicochemical and biological parameters. The objective is to support data-driven decision-making for environmental monitoring and public health planning.

---

## Project Overview
- Data cleaning and preprocessing of real-world environmental data
- Exploratory Data Analysis (EDA) to identify trends, anomalies, and class imbalance
- Feature selection using domain knowledge and statistical evidence
- Multiclass classification modeling
- Interpretation of results from a public health perspective

---

## Dataset Description
- **Source:** National Water Monitoring Programme (NWMP), Maharashtra Pollution Control Board (MPCB)
- **File:** https://drive.google.com/file/d/1ipLIVY0T0buaMl4U-Goo5e6KKWpixoL3/view
- **Rows:** ~225 (after cleaning: ~170)
- **Columns:** 54 (metadata + physicochemical & biological parameters)
- **Target Variable:** Water Quality Class (A, B, C, E)

### Water Quality Classes
| Class | Designated Use | Key Characteristics |
|------|---------------|--------------------|
| A | Drinking water (after disinfection) | Low BOD, high DO, low coliform |
| B | Outdoor bathing | Moderate BOD/DO |
| C | Drinking water (after treatment) | Higher BOD tolerated |
| E | Irrigation & industrial use | Relaxed standards |

---

## Data Cleaning & Preprocessing
Key preprocessing steps included:
- Standardizing column names
- Dropping empty, redundant, and low-information columns
- Converting numeric-like object values (e.g., `5(BDL)`, `ND`) to numeric
- Handling missing values using **median imputation**, justified through skewness analysis
- Cleaning and extracting target labels (A–E) from descriptive text
- Removing records with missing or invalid target labels
- Encoding the target variable for modeling

The final dataset was validated to ensure **no remaining missing values**.

---

## Exploratory Data Analysis (EDA)

### Key Findings
- **Severe class imbalance:** Class A accounts for ~80% of samples
- **Strong overlap between classes:** No single parameter cleanly separates all classes
- **Dissolved Oxygen:** Important indicator but highly overlapping across categories
- **BOD & COD:** Strong right-skewness with extreme pollution outliers
- **Coliform indicators:** More discriminative but still overlapping
- **pH:** Narrow regulated range; weak standalone discriminator
- **Mineral indicators (Conductivity, Hardness, Alkalinity):** Show extreme skewness and episodic pollution

EDA confirmed the need for **multivariate, non-linear, and class-aware models**.

---

## Feature Selection
Feature selection combined:
- Domain knowledge (water quality standards)
- Correlation and class-separation analysis
- Multicollinearity checks (VIF < 5)

### Final Feature Set
- pH  
- Temperature  
- Dissolved Oxygen  
- COD  
- Fecal Coliform  
- Nitrate  
- Turbidity  
- Conductivity  
- Hardness (CaCO₃)  
- Total Alkalinity  

---

## Modeling & Evaluation

### Models Trained
1. **Multinomial Logistic Regression (Baseline)**
2. **Class-weighted Random Forest**
3. **SMOTE + Random Forest (Evaluated for comparison)**

### Evaluation Strategy
- Stratified train-test split
- Stratified 5-fold cross-validation
- **Primary metric:** Macro F1-score (to address class imbalance)

---

### Model Performance Summary

| Model | Accuracy | Mean CV Macro F1 | Key Observation |
|------|---------|------------------|----------------|
| Logistic Regression | 0.57 | 0.37 | Unstable, sensitive to class overlap |
| Random Forest | 0.83 | **0.47** | Best balance without synthetic data |
| SMOTE + Random Forest | 0.89 | 0.55 | Higher score but risk of synthetic bias |

---

## Final Model Selection

**Final Model Chosen: Class-weighted Random Forest**

Although SMOTE improved Macro F1-score, it was **not selected as the final model** due to:
- Extremely small real sample sizes for minority classes
- Risk of overfitting to synthetic observations
- Reduced interpretability for public health decision-making

The **Random Forest model** was chosen as it provides a **more realistic and robust representation** of water quality patterns under real-world data constraints.

---

## Feature Importance (Random Forest)

| Feature | Importance |
|--------|-----------|
| Total Alkalinity | 0.178 |
| Hardness (CaCO₃) | 0.136 |
| Conductivity | 0.126 |
| COD | 0.108 |
| Fecal Coliform | 0.100 |
| Temperature | 0.100 |
| pH | 0.072 |
| Turbidity | 0.063 |
| Nitrate | 0.061 |
| Dissolved Oxygen | 0.056 |

**Insight:**  
Water quality classification is strongly influenced by **mineral composition**, **organic pollution**, and **microbial contamination**, rather than a single regulatory threshold.

---

## Conclusion & Public Health Insights
This project demonstrates that water quality classification depends on **interacting chemical, biological, and mineral indicators**, not isolated thresholds. While class imbalance limits predictive performance for rare categories, a class-weighted Random Forest provides a stable and interpretable solution suitable for environmental monitoring and public health analysis. The study highlights the importance of cautious model selection when working with limited and imbalanced real-world data.

---

