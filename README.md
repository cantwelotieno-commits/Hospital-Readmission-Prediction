# 🏥 Hospital Readmission Prediction
### Predicting 30-Day Diabetic Patient Readmissions Using Machine Learning

---

## 📌 Project Overview

Hospitals track **30-day readmission** as a key quality indicator. When a patient returns within 30 days of discharge, it often signals something went wrong — a medication issue, incomplete treatment, or poor discharge planning.

This project uses **10 years of real patient data** from 130 US hospitals to predict which diabetic patients are at high risk of readmission within 30 days — giving care teams a chance to intervene before the patient walks out the door.

**Dataset:** [Diabetes 130-US Hospitals (UCI Machine Learning Repository)](https://archive.ics.uci.edu/ml/datasets/diabetes+130-us+hospitals+for+years+1999-2008)
- 101,766 patient visits
- 50 columns covering demographics, diagnoses, medications, and lab results

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Python (Pandas, Matplotlib, Seaborn) | Data cleaning, EDA, visualisation |
| SQL (SQLite via Python) | Business questions & analytical queries |
| Scikit-learn | Logistic Regression, Random Forest |
| XGBoost | Best-performing model (highest recall) |
| SHAP | Model explainability |
| Power BI | Dashboard for clinical staff |
| Joblib | Model saving & export |

---

## 📂 Project Structure

```
hospital-readmission-predictor/
│
├── Hospital_Readmission_Prediction.ipynb   # Main notebook (full pipeline)
├── outputs/
│   ├── cleaned_data.csv                    # Cleaned dataset for Power BI
│   ├── readmission_model.pkl               # Saved XGBoost model
│   └── shap_importance.csv                 # Feature importance for dashboard
└── README.md
```

---

## 🔄 Project Workflow

### Step 1 — Data Understanding
Explored 101,766 patient records. Identified two types of missing values: blank cells and `?` coded missings. Noted class imbalance: only 11% of patients were readmitted within 30 days.

### Step 2 — Data Cleaning (7 deliberate decisions)
- Dropped `weight` (97% missing) and `payer_code` (not clinically relevant)
- Removed 3 invalid gender rows
- Replaced `?` with `NaN` across all columns
- Filled missing values based on clinical logic (e.g. `"Not Tested"` for lab columns rather than dropping rows)
- Translated numeric admission and discharge codes into readable labels
- Removed duplicate patients (kept first visit only)
- Simplified target: `1` = readmitted within 30 days, `0` = everything else

### Step 3 — Exploratory Data Analysis
5 visualisations exploring age distribution, readmission by discharge type, medication patterns, lab test coverage, and admission type breakdown.

### Step 4 — SQL Analysis
Answered 5 business questions using SQL queries run directly in Python:
- Which discharge destinations carry the highest readmission risk?
- Does medication change at discharge increase readmission?
- Which age groups are most at risk?
- Do patients who had A1C tested have lower readmission rates?
- Which medical specialties see the most readmissions?

### Step 5 — Feature Engineering & Preparation
- Grouped ICD-9 diagnosis codes into clinical categories
- Created a `high_risk_age` binary flag for patients aged 70–90
- Encoded categorical variables
- Applied SMOTE oversampling to handle the 11% minority class

### Step 6 — Model Training & Comparison

| Model | Recall (High Risk) | ROC-AUC | Overall Accuracy |
|---|---|---|---|
| Logistic Regression (baseline) | 26% | 0.52 | 73% |
| Random Forest | 6% | 0.58 | 88% |
| **XGBoost** | **62%** | **0.56** | **48%** |

**XGBoost was selected** as the final model. In a hospital setting, catching high-risk patients matters more than overall accuracy — a model that flags 62 out of every 100 high-risk patients is far more clinically useful than one that misses 94 of them.

### Step 7 — SHAP Explainability
Used SHAP values to explain what drives each prediction. Top findings:
1. **Medication changes** during the visit are the strongest predictor of readmission
2. The engineered `high_risk_age` feature outperformed raw age
3. Discharge disposition remains a significant risk signal
4. Diagnosis groups (ICD-9 clusters) all contributed meaningfully

> **Clinical implication:** A structured medication review at discharge — particularly for insulin and metformin — could be one of the most effective interventions for reducing 30-day readmissions.

### Step 8 — Outputs Saved
Three files exported for real-world handoff:
- `cleaned_data.csv` → BI team for Power BI dashboard
- `readmission_model.pkl` → Engineering team for deployment
- `shap_importance.csv` → Clinical team for risk factor review

---

## 📊 Power BI Dashboard

The cleaned dataset and SHAP importance table were loaded into Power BI to produce an interactive clinical dashboard showing:
- Readmission rates by age group, discharge destination, and admission type
- Top medication risk factors
- Feature importance visualisation from SHAP

*(Screenshots coming soon)*

---

## 💡 Key Insights

1. Medication management at discharge is the most powerful predictor of 30-day readmission
2. Patients aged 70–90 carry significantly elevated risk — especially when discharged home without support
3. Patients discharged to skilled nursing facilities have lower readmission rates than those discharged home alone
4. Whether a patient had their A1C tested is itself a meaningful signal — patients who were tested had lower readmission rates

---

## 👤 About the Author

**Cantwel Njeri Otieno** — Data Analyst | Nairobi, Kenya

- 🔗 [LinkedIn](https://www.linkedin.com/in/njery-cantwel/)
- 🐙 [GitHub](https://github.com/cantwelotieno-commits)
- 📧 cantwel.otieno@gmail.com

---

## 📄 Dataset Citation

Strack, B., DeShazo, J.P., Gennings, C., et al. (2014). Impact of HbA1c Measurement on Hospital Readmission Rates: Analysis of 70,000 Clinical Database Patient Records. *BioMed Research International*.
