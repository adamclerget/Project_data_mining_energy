# Building Energy Performance Prediction

## 1. Project Overview

This project investigates machine learning approaches for predicting **building energy efficiency** as a **regression task**. The objective is to estimate a continuous EPC-related energy performance score from structural, thermal, and system-level building characteristics derived from Energy Performance Certificate data.

The project has clear practical value for:

- energy optimization and retrofit targeting
- policy support for improving inefficient housing stock
- sustainability analysis
- real-estate and housing-market insight generation

---

## 2. Dataset

The project uses a processed EPC dataset for **Manchester (UK)**.

After cleaning, deduplication, leakage prevention, and feature engineering, the final modeling dataset contains approximately **266,000 observations**.

Main feature groups include:

- `construction_year_estimated`
- `feature_building_age`
- heating system type and fuel source
- roof insulation indicators and thickness
- wall insulation and wall type indicators
- glazing quality and window extent
- total floor area
- property type and building form

Source files include:

- `manchester_epc_legal.parquet`
- `manchester_epc_prepped.parquet`
- `manchester_epc_phase3_final.parquet`

---

## 3. Problem Definition

The main task is **regression**, with the goal of predicting a continuous **current energy efficiency score** from physical building attributes.

While an auxiliary binary framing (`target_is_efficient`) exists for interpretation and threshold-based discussion, the core project focuses on regression because it preserves finer-grained information and supports more precise comparison between models.

---

## 4. Methodology / Pipeline

The workflow follows a standard data mining and machine learning pipeline:

1. **Data preparation**  
   Raw EPC records are cleaned and relevant variables are selected.

2. **Feature engineering**  
   Domain-informed variables are created, especially around age, insulation, glazing, heating systems, and dwelling structure.

3. **Train/test split**  
   A holdout test split is created with `test_size = 0.2` and `random_state = 42`.

4. **Model training and tuning**  
   Both interpretable and non-symbolic models are trained and tuned.

5. **Downstream evaluation and explainability**  
   The final model set is evaluated using holdout metrics, 10-fold cross-validation, residual analysis, SHAP, feature importance, and robustness checks for older buildings.

---

## 5. Models Used

The final comparison includes:

- **DummyRegressor** baseline
- **Decision Tree** as the symbolic / interpretable model
- **Random Forest**
- **XGBoost**
- **LightGBM**
- **MLPRegressor** neural network

The strongest final models are the boosted tree methods, with XGBoost achieving the best overall performance.

---

## 6. Final Evaluation Setup

The repository now reflects the **latest iteration** of the project, including retraining after feature updates and explicit analysis of older-building prediction difficulty.

Final evaluation uses:

- **Holdout test evaluation**
- **10-fold cross-validation**
- Metrics:
  - **RMSE**
  - **MAE**

Additional downstream evaluation includes:

- residual analysis for XGBoost
- top-20 largest-error analysis
- SHAP explainability
- older-building robustness comparison
- comparison between earlier and updated evaluation versions

---

## 7. Final Results

### Holdout Test Performance

Latest updated holdout results:

| Model | RMSE | MAE |
|---|---:|---:|
| XGBoost | 5.30 | 3.57 |
| LightGBM | 5.31 | 3.59 |
| MLP | 5.69 | 3.89 |
| Decision Tree | 5.80 | 3.91 |
| DummyRegressor | 11.63 | 8.81 |

### 10-Fold Cross-Validation

| Model | RMSE Mean | RMSE Std | MAE Mean | MAE Std |
|---|---:|---:|---:|---:|
| XGBoost | 5.309 | 0.031 | 3.569 | 0.015 |
| LightGBM | 5.325 | 0.038 | 3.601 | 0.019 |
| MLP | 5.716 | 0.057 | 3.916 | 0.037 |
| Decision Tree | 5.784 | 0.038 | 3.894 | 0.021 |
| DummyRegressor | 11.645 | 0.082 | 8.801 | 0.038 |

### Main Performance Conclusion

- **XGBoost remains the best model**
- **LightGBM is extremely close behind**
- **MLP improves over the symbolic Decision Tree in predictive accuracy**
- **Decision Tree remains useful mainly for interpretability**
- **DummyRegressor confirms that all learned models capture substantial predictive structure**

---

## 8. Iteration / What Changed in the Updated Version

The latest version of the project includes an additional modeling iteration compared with the earlier evaluation snapshot.

Main changes:

- `construction_year_estimated` was kept in the final feature set alongside `feature_building_age`
- models were retrained on the updated Phase 3 dataset
- a dedicated **older-buildings analysis** was added
- a new **MLP model** was introduced
- downstream evaluation was rerun with updated outputs
- a final **10-fold validation** was added

Result of the iteration:

- all main overlapping models improved slightly
- XGBoost still ranked first
- older-building robustness improved a little, but older buildings remain the hardest subgroup

---

## 9. Error Analysis

Residual analysis was performed for the best model, **XGBoost**.

Key findings:

- the mean residual is close to zero, indicating **little overall bias**
- the largest errors are still much larger than the average error
- most extreme errors remain **overpredictions**
- older buildings are still more difficult to predict than newer buildings

For the updated version, the XGBoost age-group breakdown shows that:

- **Pre-1919** buildings are the hardest group
- **2001+** buildings are the easiest group

This suggests that historical housing stock remains more heterogeneous and less predictable, even after the feature update.

---

## 10. Explainability & Feature Impact

Explainability was studied using:

- tree-based feature importance
- SHAP values
- residual and subgroup error analysis

Important features in the final updated version include:

- `wall_insulation_none`
- `roof_insulation_thickness_mm`
- `construction_year_estimated`
- `feature_building_age`
- `win_glazing_high_perf`
- heating-system variables
- total floor area

This confirms that **building envelope quality, age, and heating system characteristics** are central drivers of energy performance prediction.

---

## 11. Interpretable Model

The Decision Tree serves as the project’s **symbolic / interpretable model**.

It was used to:

- visualize human-readable decision rules
- inspect feature importance
- compare an interpretable model against stronger non-symbolic models

Although less accurate than XGBoost and LightGBM, it remains useful for explaining broad structural relationships in the data.

---

## 12. Repository Structure

For final submission, the repository is organized so that notebooks, source data, trained models, and evaluation outputs are separated clearly.

### Root

Main items kept in the root directory:

- notebooks
  - `data_prep.ipynb`
  - `02_feature_engineering.ipynb`
  - `03_decision_tree.ipynb`
  - `04_non_symbolic-v2.ipynb`
- `README.md`
- source datasets (`*.parquet`)
- report folder if included separately

### `models/`

Trained model files are stored here, including:

- `dt_best_model.pkl`
- `xgb_best_model.pkl`
- `lgb_best_model.pkl`
- `mlp_best_model.pkl`
- any additional saved model artifacts such as Random Forest if present

### `evaluation_outputs/`

Generated evaluation outputs are stored here, including:

- holdout prediction files
- updated `_v2` evaluation files
- `10fold` cross-validation files
- old-vs-new comparison tables
- SHAP outputs
- residual plots
- top-20 error analysis files
- feature importance plots and summary tables

---

## 13. Conclusion

This project shows that machine learning models, especially **gradient-boosted tree methods**, can predict building energy performance with strong accuracy from engineered EPC features.

The final updated evaluation confirms that:

- XGBoost is the strongest overall model
- LightGBM is a very close second
- the feature update improved performance slightly
- older buildings remain the most difficult subgroup
- age, insulation, glazing, and heating-system features are consistently the most influential predictors

Overall, the project combines predictive performance, interpretability, robustness analysis, and practical relevance for energy-efficiency assessment and housing-policy applications.
