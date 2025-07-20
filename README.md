# Predicting Hospital Disposition for EMS Patients with Suspected Opioid Overdose

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Data Sources](#data-sources)
  - [Terms and Conditions](#terms-and-conditions)
  - [Dataset Details](#dataset-details)
- [Methodology](#methodology)
  - [Data Preparation](#data-preparation)
  - [Modeling](#modeling)
  - [Evaluation & Interpretability](#evaluation--interpretability)
- [Recommendations, Conclusions & Future Work](#recommendations-conclusions--future-work)
- [Environment & Reproducibility](#environment--reproducibility)

---

## Project Overview

The opioid epidemic continues to claim lives across the United States, with emergency medical services (EMS) frequently serving as the first line of intervention. This project explores whether EMS documentation — specifically data submitted to the National EMS Information System (NEMSIS) — contains predictive signals that can help identify which patients are likely to be admitted to the hospital following an opioid overdose.

By using machine learning on EMS reports and known hospital outcomes, we aim to surface actionable insights that can support training, education, public health initiatives, and cross-agency coordination.

---

## Objectives

- Use supervised machine learning to predict ED disposition (admitted vs discharged) for opioid-related EMS incidents.
- Demonstrate that prehospital EMS data contains meaningful features correlated with hospital outcomes.
- Encourage greater integration of hospital outcome data with EMS records to unlock new opportunities for public health and education.

---

## Data Sources

This project used the **2023 NEMSIS Public Release Research Dataset**, available by request from the NEMSIS Technical Assistance Center:

- Dataset Portal: [https://nemsis.org/2022-nemsis-public-release-research-dataset-now-available/](https://nemsis.org/2022-nemsis-public-release-research-dataset-now-available/)
- Data Dictionary: [NEMSIS v3.4.0 Data Dictionary](https://nemsis.org/media/nemsis_v3/release-3.4.0/DataDictionary/PDFHTML/DEMEMS/index.html)

### Terms and Conditions

Use of the NEMSIS dataset is subject to strict conditions outlined by the National Highway Traffic Safety Administration (NHTSA), including:

- Use is permitted for research, advocacy, education, or EMS-related nonprofit efforts only.
- The dataset is **not population-based** and must be cited as follows:

  > National Highway Traffic Safety Administration, National Emergency Medical Services Information System. The content reproduced from the NEMSIS Database remains the property of the National Highway Traffic Safety Administration.

- Users may not publish or redistribute the dataset. A copy of this project includes no original data to comply with this agreement. All users must request access independently.
- Full terms: See dataset page linked above.

### Dataset Details

- The full uncompressed dataset is approximately **160GB**, containing millions of EMS activations submitted by U.S. states and agencies.
- Our subset filters for incidents involving **suspected opioid overdose** where hospital outcome data (ED disposition) was recorded.
- Final sample size for supervised modeling: **5,677 records** (0.01% of total calls in the full dataset).

---

## Methodology

### Data Preparation

- Parsed and filtered structured pipe-delimited files using custom scripts.
- Mapped coded variables using official NEMSIS codebooks.
- Engineered relevant features such as vitals, protocol usage, symptom counts, and more.
- Imputed missing values with logical defaults (e.g., median vitals, flag defaults).
- Created binary classification target: **Admit/Transfer/Dead** vs **Discharged/AMA**.
- Applied stratified train-test split (80/20).

### Modeling

- Built a baseline **logistic regression** classifier.
- Trained and tuned an **XGBoost classifier** using randomized hyperparameter search.
- Tuned model achieved:
  - Accuracy: **0.83**
  - Recall (Class 1 - Admitted): **0.81**
  - ROC AUC: **0.91**

### Evaluation & Interpretability

- Evaluated performance using precision, recall, F1-score, and confusion matrix.
- Used **SHAP values** and feature importance plots to understand model drivers.
- Key predictive features included:
  - Specific EMS protocols used
  - Primary provider impression codes (ICD-10)
  - Cardiac arrest events
  - Patient age and payment method
  - Systolic blood pressure and heart rate

---

## Recommendations, Conclusions & Future Work

- This project demonstrates that **EMS reports contain predictive signals** about patient outcomes — particularly when detailed protocols, vitals, and impressions are documented.
- The models built here could help EMS systems identify cases for follow-up, drive protocol review, or support public health research.
- However, only a tiny fraction (~0.01%) of calls in the full dataset included a confirmed ED outcome. **Expanding hospital data sharing** is essential.
- As of this writing, the **2024 NEMSIS datasets** have been released. Adding these to the project is on the roadmap to improve data volume and model performance.
- We urge EMS agencies, public health departments, and hospitals to strengthen outcome data reporting and integration to help save lives and address this epidemic from all angles.

---

## Environment & Reproducibility

This project was developed using a custom **conda environment**. A full `environment.yml` file is included for reproducibility. Key packages include:

- `pandas`, `numpy`
- `xgboost`
- `scikit-learn`
- `matplotlib`, `seaborn`
- `shap`

To recreate the environment:

```bash
conda env create -f environment.yml
conda activate nemsis-env
