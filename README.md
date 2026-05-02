# Building Energy Performance Prediction

## 1. Project Overview

This project investigates the use of machine learning to predict building energy efficiency as a regression task. The objective is to estimate a continuous building energy performance score from structural, thermal, and system-related characteristics derived from Energy Performance Certificate (EPC) data.

The project has strong practical relevance for energy optimization, public policy design, and real-estate decision making. Accurate prediction of building energy performance can help identify inefficient properties, support retrofit prioritization, and provide useful insights for sustainability planning.

---

## 2. Dataset

The project is based on an EPC dataset for **Manchester (UK)**, processed into a modeling-ready tabular dataset.

After cleaning, deduplication, feature engineering, and leakage prevention, the final dataset contains approximately **266,000 observations**.

The features describe building characteristics that influence energy performance, including:

- Estimated building age
- Heating system type and energy source
- Roof and wall insulation indicators
- Glazing quality and window extent
- Total floor area
- Property type and building form

---

## 3. Problem Definition

The main task is a **regression problem**, where the goal is to predict a continuous building energy efficiency score.

The target variable represents energy performance in EPC data (linked to kWh/m²/year consumption levels).

While a classification framing (efficient vs inefficient) is possible, this project focuses on regression to preserve detailed information and allow more precise modeling.

---

## 4. Methodology / Pipeline

The project follows a standard end-to-end data mining workflow:

1. **Data preprocessing**  
   Raw EPC data is cleaned, categorical text fields are parsed, and relevant variables are extracted.

2. **Feature engineering**  
   Domain-informed features are created, including building age estimation, insulation indicators, heating system grouping, and structural attributes.

3. **Train/Test Split**  
   A holdout test set is created using an 80/20 split (`test_size = 0.2`, `random_state = 42`) to evaluate generalization performance.

4. **Modeling**  
   Both interpretable and non-symbolic models are trained and compared.

---

## 5. Models Used

The following models were evaluated:

- **DummyRegressor** (baseline)
- **Decision Tree** (interpretable / symbolic model)
- **Random Forest** (ensemble baseline, included in evaluation results)
- **XGBoost** (best-performing model)
- **LightGBM** (high-performance alternative)

Hyperparameters were tuned using grid search and cross-validation. Example tuning dimensions include:

- Tree depth (`max_depth`)
- Minimum samples per leaf (`min_samples_leaf`)
- Learning rate (boosting models)
- Number of estimators

---

## 6. Evaluation Setup

Model performance was evaluated using:

- **Train/Test split (holdout evaluation)**
- **5-fold cross-validation**
- Metrics:
  - RMSE (Root Mean Squared Error)
  - MAE (Mean Absolute Error)

Stratification was not applied, as the task is a continuous regression problem. A cost matrix was also not used, as it is more relevant for classification tasks.

Evaluation was conducted systematically across all models using the same dataset and metrics.

---

## 7. Results

### Test Set Performance

| Model | RMSE | MAE |
|------|-----:|----:|
| XGBoost | ~5.41 | ~3.66 |
| LightGBM | ~5.42 | ~3.68 |
| Decision Tree | ~5.87 | ~3.97 |
| DummyRegressor | ~11.63 | ~8.81 |

### Cross-Validation (5-fold)

- XGBoost: RMSE ≈ 5.42 (low variance)
- LightGBM: RMSE ≈ 5.44
- Decision Tree: RMSE ≈ 5.90

XGBoost consistently achieved the best performance, with LightGBM performing very closely behind.

---

## 8. Error Analysis

Residual analysis was conducted for the best-performing model (XGBoost):

- The mean residual is close to zero → **no strong overall bias**
- The largest errors are significantly higher than average
- Most extreme errors are **overpredictions**
- Errors are more frequent for **older buildings**

This indicates that while the model performs well overall, a subset of properties remains difficult to predict accurately.

---

## 9. Explainability & Feature Impact

Model behavior was analyzed using:

- Feature importance (tree-based models)
- SHAP (SHapley Additive Explanations)
- Residual plots

Key influential features include:

- Insulation quality (walls and roof)
- Glazing quality
- Building age
- Heating system characteristics
- Floor area

These results are consistent with domain knowledge and confirm that building envelope properties strongly influence energy performance.

---

## 10. Interpretable Model (Decision Tree)

The Decision Tree model provides human-readable decision rules. For example:

- Buildings with **poor insulation and older construction age** tend to have higher predicted energy consumption
- Buildings with **modern glazing and efficient heating systems** show significantly better energy performance

Although less accurate than boosting models, the Decision Tree helps interpret the relationship between features and energy efficiency.

---

## 11. Repository Contents

The repository contains:

### Notebooks
- `data_prep.ipynb`
- `02_feature_engineering.ipynb`
- `03_decision_tree.ipynb`
- `04_non_symbolic.ipynb`

### Models
- `dt_best_model.pkl`
- `xgb_best_model.pkl`
- `lgb_best_model.pkl`

### Evaluation Outputs
- `predictions.parquet`
- `evaluation_results.csv`
- `cv_results.csv`

### Explainability Outputs
- SHAP plots and values
- Feature importance plots
- Residual analysis plots
- Error analysis tables

### Data
- Processed EPC datasets (parquet format)
- Test split (`X_test`, `y_test`)

---

## 12. Conclusion

This project demonstrates that machine learning models, particularly gradient-boosted tree methods, can predict building energy performance with strong accuracy using engineered EPC features.

The results highlight the importance of structural and thermal building characteristics such as insulation, glazing, age, and heating systems.

From a practical perspective, the model can support:

- Energy efficiency analysis
- Retrofit prioritization
- Policy-making decisions
- Real-estate valuation insights

Overall, the project successfully combines predictive performance with interpretability and real-world applicability.
