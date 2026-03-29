# Phase-III-Trial-Data-Analysis-and-Predictive-modeling.ipynb
Exploratory analysis on survival rate and adverse effect outcomes using Phase 3 Trial data and uilding predictive machine learning models.
# Exploratory Analysis of Phase III Trial Data
## Survival Outcomes and Adverse Event Profiling

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Lifelines](https://img.shields.io/badge/lifelines-0.27+-green)
![Scikit-learn](https://img.shields.io/badge/scikit--learn-1.3+-orange)
![Status](https://img.shields.io/badge/status-complete-brightgreen)

---

## Overview

This project replicates the kind of exploratory survival analysis performed daily by clinical data teams at CROs and pharmaceutical companies. Using the publicly available **NCCTG Lung Cancer dataset** (Loprinzi et al., *Journal of Clinical Oncology*, 1994), the analysis covers survival curve estimation, statistical significance testing, adverse event profiling, and predictive modelling — all implemented in Python.

The dataset contains 228 patients from a real Phase III lung cancer trial, with survival times, censoring indicators, performance scores, and adverse event markers.

---

## Key Findings

| Analysis | Result |
|---|---|
| Overall median survival | 255.5 days (~8.5 months) |
| Median survival — Male | 270 days |
| Median survival — Female | 426 days |
| Sex difference (log-rank) | **p = 0.0013 ✓ Significant** |
| Median survival — ECOG 0 | Longest |
| Median survival — ECOG 2 | Shortest |
| ECOG vs survival (log-rank) | **p = 0.0093 ✓ Significant** |
| Weight loss vs survival (log-rank) | p = 0.6055 ✗ Not significant |
| Best survival prediction model | Linear Regression (lowest CV MAE) |
| Best weight loss classifier | Gradient Boosting (55.9% accuracy) |

---

## Project Structure

```
├── Phase_III_Survival_Analysis.ipynb   # Main analysis notebook
└── README.md
```

---

## Analysis Components

### 1. Survival Analysis (Kaplan-Meier)
- Overall cohort survival curve with 95% confidence intervals
- Sex-stratified survival curves
- ECOG performance score-stratified curves
- Log-rank tests for all group comparisons

### 2. Adverse Event Profiling
- Weight loss categorised by severity (None/Gain, Mild, Moderate, Severe)
- Frequency distribution across severity categories
- Survival curves stratified by AE severity
- Statistical testing for AE severity vs survival relationship

### 3. Predictive Modelling
- **Task 1 — Regression:** Predict survival time from patient characteristics
- **Task 2 — Classification:** Predict weight loss severity category
- Five models compared per task: Linear/Logistic Regression, Random Forest, Gradient Boosting, XGBoost, SVM, KNN
- Hyperparameter tuning via GridSearchCV
- 5-fold cross-validation for robust performance estimation
- Feature importance analysis
- Data leakage identification and correction

---

## Methods

| Method | Purpose |
|---|---|
| Kaplan-Meier estimation | Survival curve generation with censoring |
| Log-rank test | Statistical comparison between survival curves |
| Multivariate log-rank test | Multi-group survival comparisons (ECOG, weight loss) |
| GridSearchCV | Systematic hyperparameter optimisation |
| 5-fold cross-validation | Robust model evaluation on small dataset |
| Feature engineering | Karnofsky score gap, age × ECOG interaction term |

---

## Libraries

```python
lifelines       # Survival analysis
scikit-learn    # Machine learning models and evaluation
xgboost         # Gradient boosting
pandas          # Data manipulation
numpy           # Numerical operations
matplotlib      # Visualisation
seaborn         # Statistical visualisation
```

---

## Dataset

**NCCTG Lung Cancer Dataset**
- Source: North Central Cancer Treatment Group
- Publication: Loprinzi et al. *Journal of Clinical Oncology* 12(3):601–7, 1994
- Access: Built into the `lifelines` Python library (`from lifelines.datasets import load_lung`)
- 228 patients | 165 events (death) | 63 censored

**Key variables:**

| Variable | Description |
|---|---|
| `time` | Survival time (days) |
| `status` | 1 = censored, 2 = dead |
| `sex` | 1 = Male, 2 = Female |
| `ph.ecog` | ECOG performance score (0–3) |
| `ph.karno` | Karnofsky score (physician-rated) |
| `pat.karno` | Karnofsky score (patient-rated) |
| `meal.cal` | Calories consumed at meals |
| `wt.loss` | Weight loss in last 6 months (lbs) |

---

## Model Performance Summary

### Survival Time Prediction (Regression)
All models showed consistently high cross-validated MAE, reflecting the limited predictive power of demographic and functional features alone. Linear Regression outperformed ensemble methods — consistent with known overfitting risk on small datasets (n < 300).

**Clinical note:** Precise survival prediction requires tumour stage, treatment type, and molecular markers not available in this dataset. This finding aligns with established limitations in clinical ML literature.

### Weight Loss Category Prediction (Classification)
Gradient Boosting achieved 55.9% accuracy across four severity categories vs a 25% random baseline — demonstrating moderate signal in available features.

### Data Leakage Note
Weight loss (`wt.loss`) was initially included as a regression feature but identified as a source of data leakage — it represents a future outcome rather than a baseline predictor. Its removal improved model generalisability and is documented in the notebook.

---

## Limitations

- Weight loss used as proxy AE marker (no dedicated adverse event table in this dataset)
- `meal.cal` missing in ~20% of patients (47/228)
- Small subgroup sizes limit statistical power for AE-stratified survival analysis
- Predictive models limited by available feature set — tumour stage and treatment data not included
- Single-institution data may limit generalisability

---

## How to Run

1. Open [Google Colab](https://colab.research.google.com)
2. Upload `Phase_III_Survival_Analysis.ipynb`
3. Run all cells in order — all dependencies install automatically in Cell 1

No local installation required.

---

## Context

This project was built as part of a data science portfolio targeting clinical research and pharmacovigilance roles. The methodology — survival analysis, adverse event profiling, and statistical significance testing — reflects standard analytical workflows used by CRO data teams in Phase II/III trial reporting.

---

## References

Loprinzi CL, Laurie JA, Wieand HS, et al. (1994). Prospective evaluation of prognostic variables from patient-completed questionnaires. *Journal of Clinical Oncology*, 12(3), 601–607.
