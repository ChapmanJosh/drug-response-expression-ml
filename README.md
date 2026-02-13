# Cancer Drug Response Prediction from Gene Expression

## Goal
Build a reproducible machine learning pipeline that predicts **drug response** (e.g., IC50/AUC) from **gene expression** data, suitable for a biotech ML portfolio.

## What this repo will include
- Clean train/val/test splitting strategy (leakage-aware)
- Baseline models (Elastic Net / Random Forest)
- Strong models (XGBoost or similar)
- Model interpretability (e.g., feature importance / SHAP)
- Reproducible training + inference scripts
- (Later) a small demo app/API for inference

## Repository structure
- data/ — **ignored by Git** (raw/processed datasets stay local)
- 
otebooks/ — EDA notebooks (keep them tidy and reproducible)
- src/ — reusable Python modules (data prep, features, models)
- configs/ — config files for experiments
- eports/figures/ — plots and figures for README/report
- models/ — **ignored by Git** (saved models stay local)
- pp/ — (later) API/UI demo
- 	ests/ — lightweight tests

## Quickstart (local)
1. Create and activate a virtual environment
2. Install dependencies from equirements.txt
3. Add data following data/README.md
4. Run training (to be implemented):  
   python -m src.models.train --config configs/default.yaml

## Notes
This repo is currently **local-only** (not on GitHub yet). The structure is designed to publish cleanly later.
