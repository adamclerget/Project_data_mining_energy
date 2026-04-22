## Phase 4 — Non-symbolic Models

Three ensemble models are trained in `04_non_symbolic.ipynb`: 
Random Forest, XGBoost, LightGBM.

### Model files

- `xgb_best_model.pkl` — included
- `lgb_best_model.pkl` — included
- `rf_best_model.pkl` — **not included** (exceeds GitHub file size limit)

To regenerate the Random Forest model, run section 5 of 
`04_non_symbolic.ipynb`. The tuning step takes approximately 30–60 minutes.

All CV results are recorded in `phase4_nonsymbolic_results.parquet`, 
so Random Forest metrics are preserved even without the model file.
