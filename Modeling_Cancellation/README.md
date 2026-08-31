# Modeling_Cancellation

This folder contains notebooks and scripts for building, evaluating, and exporting models that predict hotel booking cancellations.

## Overview

The goal of this work is to predict whether a booking will be cancelled (binary classification) based on reservation, customer and property features. The notebooks show the full modelling workflow from exploratory data analysis and feature engineering to model training, evaluation, and exporting reproducible artifacts.

## Contents

- `00-data-prep.ipynb` – data loading and cleaning, feature extraction and preprocessing pipelines.
- `01-exploratory-analysis.ipynb` – EDA, visualisations, and initial feature assessments.
- `02-modeling.ipynb` – model training (baseline models, hyperparameter tuning), cross-validation and calibration.
- `03-evaluation-and-interpretation.ipynb` – metrics, confusion matrices, ROC/PR curves, and model interpretation (feature importance, SHAP).
- `utils/` – helper functions and reusable preprocessing/modeling utilities.
- `models/` – saved model artifacts and serialized pipelines (created by the training notebooks).

(If your filenames differ, update this list to match the actual files in the directory.)

## Data

Expect the raw dataset to be placed in `../data/` or a location configured in the notebooks (e.g. `../data/hotel_bookings.csv`). Notebooks start from the cleaned dataset produced by `00-data-prep.ipynb` unless noted otherwise.

Key assumptions:
- Target column: `is_canceled` (0 = not cancelled, 1 = cancelled).
- Typical features: booking lead time, arrival date, number_of_adults/children, market segment, distribution channel, previous cancellations, deposit type, customer type, adr, etc.

## Modeling approach

- Baseline models: Logistic Regression and Decision Tree.
- Stronger models: Random Forest, XGBoost/LightGBM.
- Pipelines: Scikit-learn Pipelines are used to encapsulate preprocessing (imputation, encoding, scaling) and model training so that exported artifacts are reproducible.
- Imbalanced classes: apply resampling (SMOTE) or class weighting where appropriate; evaluate with precision-recall and ROC AUC.

## Preprocessing highlights

- Dates: extract useful features (lead time, day-of-week, month, seasonality).
- Missing values: impute numerical features with median and categorical with a special category or mode.
- Categorical encoding: use One-Hot or Target/Ordinal encoding depending on cardinality.
- Scaling: numeric features are scaled when required by the model.

## Evaluation

- Primary metrics: ROC AUC and PR AUC. Also report precision, recall, F1, and confusion matrix at chosen thresholds.
- Use stratified cross-validation and a held-out test set for final evaluation.
- Save evaluation reports and plots to `outputs/` or `reports/` for reproducibility.

## How to run

1. Create a Python environment and install dependencies (see project-level `requirements.txt` or environment file):

   pip install -r requirements.txt

2. Run the notebooks in order:
   - `00-data-prep.ipynb`
   - `01-exploratory-analysis.ipynb`
   - `02-modeling.ipynb`
   - `03-evaluation-and-interpretation.ipynb`

3. Trained models and pipelines will be saved under `models/`.

You can also run key scripts (if present) in a headless mode for reproduction; adapt the commands to the repo's structure.

## Outputs & artifacts

- Trained model files (`.pkl`, joblib, or model-specific format).
- Preprocessing pipelines and encoders.
- Evaluation reports and plots (ROC, PR curves, calibration plots).
- Feature importance tables and interpretability artifacts (SHAP values).

## Reproducibility

- Set a fixed random seed in the notebooks for deterministic behavior where possible.
- Note the environment (Python version and package versions) used to create models and provide a `requirements.txt` or `environment.yml`.

## Next steps / Improvements

- Add hyperparameter sweeps using tools such as Optuna or scikit-optimize.
- Improve temporal validation if bookings are time-dependent (use time-series cross-validation by booking date).
- Build a lightweight inference service for batch scoring or integrate into a dashboard for monitoring cancellations.

## Contact

If you have questions or want to contribute, open an issue or contact the repository owner.
