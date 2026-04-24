# Building Energy Performance Prediction

## 1. Project Overview

This project investigates the use of machine learning to predict building energy efficiency as a regression task. The objective is to estimate a continuous building energy performance score from structural, thermal, and system-related characteristics derived from Energy Performance Certificate (EPC) data.

The project has strong practical relevance for energy optimization, public policy design, and real-estate decision making. Accurate prediction of building energy performance can help identify inefficient properties, support retrofit prioritization, and provide useful insights for sustainability planning.

## 2. Dataset

The project is based on an EPC dataset sourced from **ADEME (France)** and processed into a modeling-ready tabular dataset. After cleaning, feature engineering, and leakage prevention, the final dataset contains approximately **266,000 rows**.

The features describe building characteristics that are expected to influence energy performance, including:

- Building age
- Heating system type and fuel source
- Roof and wall insulation indicators
- Glazing quality and window extent
- Total floor area
- Property type and built form

## 3. Problem Definition

The main task is a **regression problem**: predicting a continuous building energy efficiency score, expressed in **kWh/m2/year**.

An auxiliary binary framing of the problem can also be considered for classifying buildings into more or less efficient categories, but the primary focus of this project is regression because it preserves more information and supports finer-grained analysis.

## 4. Methodology / Pipeline

The project follows a standard end-to-end data mining workflow:

1. **Data preprocessing**
   Raw EPC data is cleaned, relevant attributes are selected, and textual descriptors are transformed into usable variables.

2. **Feature engineering**
   Domain-relevant indicators are constructed for heating systems, insulation, glazing, building form, and estimated building age.

3. **Train/test split**
   The processed dataset is split into training and test subsets to evaluate model generalization.

4. **Modeling**
   Multiple machine learning models are trained and compared, including both interpretable and high-performance ensemble approaches.

## 5. Models Used

The following models were used in the project:

- **DummyRegressor** as a simple baseline
- **Decision Tree** as an interpretable benchmark model
- **Random Forest** as an ensemble tree-based method
- **XGBoost** as the best-performing gradient boosting model
- **LightGBM** as an efficient gradient boosting alternative

The Decision Tree offers interpretability, while the boosting models provide stronger predictive performance. Random Forest results are included in evaluation outputs, although the saved model file is not included in the repository because of file-size constraints.

## 6. Evaluation Metrics

Model performance is assessed using:

- **RMSE (Root Mean Squared Error)**
- **MAE (Mean Absolute Error)**
- **5-fold cross-validation**

Approximate test-set results are:

| Model | RMSE | MAE |
| --- | ---: | ---: |
| XGBoost | ~5.41 | ~3.66 |
| LightGBM | ~5.42 | ~3.68 |
| Decision Tree | ~5.87 | ~3.97 |
| DummyRegressor | ~11.63 | ~8.81 |

The 5-fold cross-validation results confirm the same ranking, with XGBoost remaining the strongest model and LightGBM performing very closely behind it.

## 7. Explainability

Model explainability was explored through:

- **Feature importance analysis**
- **Residual analysis**
- **SHAP (SHapley Additive exPlanations)**

The explainability results show that the most influential predictors are closely linked to building envelope and thermal performance, especially:

- Insulation quality
- Glazing quality
- Building age
- Heating system characteristics
- Floor area

These findings are consistent with domain knowledge in building energy analysis.

## 8. Key Results

The main findings of the project are:

- **XGBoost achieved the best predictive performance**
- **LightGBM performed nearly as well**
- **Decision Tree provided useful interpretability but lower accuracy**
- **Building envelope features had a particularly strong impact on energy performance**

Residual analysis also showed that the best model had low overall bias, although a small number of buildings still generated large prediction errors.

## 9. Repository Contents

The repository contains the main project artifacts, including:

- **Notebooks**
  - `data_prep.ipynb`
  - `02_feature_engineering.ipynb`
  - `03_decision_tree.ipynb`
  - `04_non_symbolic.ipynb`

- **Saved models**
  - `dt_best_model.pkl`
  - `xgb_best_model.pkl`
  - `lgb_best_model.pkl`

- **Predictions and evaluation outputs**
  - `predictions.parquet`
  - `evaluation_results.csv`
  - `cv_results.csv`

- **Explainability outputs**
  - SHAP plots and SHAP values files
  - Feature importance plots and CSV summaries
  - Residual analysis plots and error tables

- **Processed datasets**
  - Final modeling and test-set parquet files

## 10. Conclusion

This project demonstrates that machine learning models, particularly gradient-boosted tree methods, can predict building energy performance with strong accuracy using engineered EPC-based features. The results also show that interpretable structural and thermal characteristics such as insulation, glazing, age, and heating systems are central drivers of model behavior.

Beyond academic relevance, the project has clear real-world value for energy-efficiency analysis, retrofit planning, environmental policy support, and data-driven real-estate assessment.
