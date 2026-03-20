# Cancer Drug Response Prediction from Gene Expression

## Goal
Build a reproducible machine learning pipeline that predicts **drug response** (e.g., IC50/AUC) from **gene expression** data, suitable for a biotech ML portfolio.

## Overview

This project predicts **compound-specific drug sensitivity** from **baseline gene expression** using DepMap/CCLE expression profiles (log2(TPM+1)) and PRISM secondary screen dose–response data. For a given compound and cell line (DepMap ID), the model predicts **AUC** as a summary of response across the tested concentration range. In the single-drug setting (**dinaciclib**), an XGBoost regressor achieved **TEST RMSE = 0.0891** and **TEST R² = 0.0810** (early-stopped at ~25 boosting rounds). To evaluate scalability, the same pipeline was applied to the **top 10 compounds by coverage**, producing a leaderboard with **mean TEST RMSE = 0.1058 ± 0.0287** and **mean TEST R² = 0.0444 ± 0.0868**. The outputs support **in-silico screening triage** (prioritizing drug–model experiments) and **biomarker hypothesis generation** via feature attribution (e.g., SHAP).

## Pipeline

```mermaid
flowchart LR
  A[DepMap Expression<br/>log2(TPM+1)] --> B[Preprocessing<br/>median impute<br/>top-variance genes (e.g., 2000)]
  C[PRISM Secondary Screen<br/>dose–response params] --> D[Target Engineering<br/>select compound<br/>AUC per cell line]
  B --> E[Merge on depmap_id<br/>cell line]
  D --> E
  E --> F[Group-aware Split<br/>by depmap_id]
  F --> G[Model<br/>Ridge / ElasticNet / XGBoost]
  G --> H[Outputs<br/>metrics (RMSE/R²)<br/>pred-vs-true plot<br/>leaderboard (top-10)]

## What this repo will include
- Clean train/val/test splitting strategy (leakage-aware)
- Baseline models (Elastic Net / Random Forest)
- Strong models (XGBoost or similar)
- Model interpretability (e.g., feature importance / SHAP)
- Reproducible training + inference scripts
- (Later) a small demo app/API for inference

## Repository structure
- `data/` — **ignored by Git** (raw/processed datasets stay local)
- `notebooks/` — EDA + training notebooks
- `src/` — reusable Python modules (data prep, features, models)
- `configs/` — config files for experiments
- `reports/figures/` — plots and figures for README/report
- `models/` — **ignored by Git** (saved models stay local)
- `app/` — (later) API/UI demo
- `tests/` — lightweight tests

## Quickstart (local)
1. Create and activate a virtual environment
2. Install dependencies from `requirements.txt`
3. Add data following `data/README.md`
4. Run notebooks in order:
   - `01_process_expression.ipynb`
   - `02_process_prism_secondary.ipynb`
   - `03_build_modeling_table.ipynb`
   - `04_train_baselines.ipynb`
   - `06_multidrug_top10_leaderboard.ipynb`

## Results (single-drug model: dinaciclib)

Target: PRISM secondary screen **AUC** (regression)  
Best model: **XGBoost** (early stopping; best iteration ≈ 23)

**Test performance**
- RMSE: 0.0898
- MAE: 0.0749
- R²: 0.0659

![Predicted vs True AUC](reports/figures/xgb_pred_vs_true_modeling_dinaciclib_auc.png)

Artifacts:
- `reports/metrics_modeling_dinaciclib_auc.json`
- `reports/metrics_modeling_dinaciclib_auc.csv`

## Multi-drug benchmark (Top 10 by coverage)

To test whether the pipeline generalizes beyond a single compound, I trained **separate XGBoost models** for the **top 10 compounds by cell-line coverage** (PRISM secondary screen AUC), using a **group-aware split by DepMap cell line (ACH-...)**.

**Aggregate performance (Top 10)**
- Mean TEST RMSE: **0.1058 ± 0.0287**
- Mean TEST R²: **0.0444 ± 0.0868**

**Leaderboard (sample)**
| Compound | TEST RMSE | TEST R² | n cell lines |
|---|---:|---:|---:|
| selinexor | 0.0561 | 0.0576 | 475 |
| tivantinib | 0.0749 | 0.0275 | 475 |
| dinaciclib | 0.0891 | 0.0810 | 476 |
| ganetespib | 0.0982 | 0.0483 | 475 |
| barasertib | 0.1001 | -0.0167 | 475 |
| tepoxalin | 0.1109 | 0.0040 | 475 |

Artifacts:
- `reports/leaderboard_top10_auc_xgb.csv`
- `reports/leaderboard_top10_auc_xgb.json`

## Notes
This repo is currently **local-only** (not on GitHub yet). The structure is designed to publish cleanly later.
