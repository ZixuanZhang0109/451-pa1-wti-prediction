# 451-pa1-wti-prediction
Week 3 Programming Assignment 1
## Author
Zixuan Zhang  
Date: July 12, 2025

## Overview
This project uses XGBoost to forecast the direction of next-day returns for WTI Crude Oil Futures. The workflow includes data retrieval, feature engineering, standardization, cross-validation, hyperparameter tuning, and evaluation.

## Files Included
- `financial_time_series.ipynb`: Jupyter Notebook with all code.
- `financial_time_series.html`: HTML export of the notebook.
- `451_pa1_report.pdf`: PDF report with analysis and results.
- `wti-with-computed-features.csv`: CSV with engineered features.

## How to Run This Project

### 1. Clone the Repository

```bash
git clone https://github.com/ZixuanZhang0109/451-pa1-wti-prediction.git
cd 451-pa1-wti-prediction
```

### 2. Set Up Python Environment

You can either use pip or conda to install dependencies.

#### Option A: Using pip
```bash
pip install -r requirements.txt
```
Or manually:
```bash
pip install yfinance polars scikit-learn xgboost matplotlib pandas
```

#### Option B: Using conda
```bash
conda create -n wti-env python=3.11
conda activate wti-env
conda install -c conda-forge polars yfinance scikit-learn xgboost matplotlib pandas
```

---

### 3. Run the Notebook

Start Jupyter Notebook:

```bash
jupyter notebook financial_time_series.ipynb
```

Then:
1. Run each cell sequentially.
2. The notebook performs:
   - Data download from Yahoo Finance (CL=F).
   - Feature engineering via Polars.
   - Descriptive statistics and EDA.
   - Standardization and XGBoost modeling.
   - Hyperparameter tuning with time-series cross-validation.
   - Model evaluation and visualization.

---

## Project Structure

| File                           | Description |
|--------------------------------|-------------|
| `financial_time_series.ipynb` | Main Jupyter notebook with code and output |
| `financial_time_series.html`  | Rendered notebook output |
| `451_pa1_report.pdf`          | Final written report for submission |
| `wti-with-computed-features.csv` | Processed dataset after feature engineering |
| `README.md`                   | This documentation file |

---

## Model Summary

- **Algorithm**: XGBoost Classifier
- **Task**: Binary classification (price up vs. down)
- **Features**: Lagged prices, HML spreads, OMC gaps, EMAs, volume lags, log returns
- **Target**: `Target = 1` if today's Close > yesterday's Close, else `0`

### Best Hyperparameters (from RandomizedSearchCV)

```text
n_estimators:      400
learning_rate:     0.1
max_depth:         3
subsample:         0.6
colsample_bytree:  1.0
min_child_weight:  1
gamma:             0.0
```

### Performance

| Metric                | Value   |
|------------------------|---------|
| Training ROC-AUC       | 0.8608  |
| Cross-Validated ROC-AUC| 0.5175  |
| Accuracy (Train)       | 0.7771  |

> The large gap between in-sample and cross-validated ROC-AUC reveals potential overfitting.

---

## Feature Engineering

Features were created using the Polars library, including:

- Lagged Close Prices: `CloseLag1`, `CloseLag2`, `CloseLag3`
- High Minus Low: `HMLLag1`, `HMLLag2`, `HMLLag3`
- Open Minus Close: `OMCLag1`, `OMCLag2`, `OMCLag3`
- Lagged Volume: `VolumeLag1`, `VolumeLag2`, `VolumeLag3`
- EMAs: `CloseEMA2`, `CloseEMA4`, `CloseEMA8`
- Log return: `LogReturn`

---

## Model Evaluation Visuals

The notebook includes:
- ROC Curve
- Confusion Matrix
- Top 10 Feature Importances

These help interpret how the model makes predictions and where it struggles (e.g., predicting rare reversals).

---

## AI Use Statement

This project includes assistance from **OpenAI ChatGPT**, used for:

- Python code generation and debugging
- Feature engineering with Polars
- Report writing and grammar polishing
- README formatting and markdown generation

All final analysis, modeling decisions, and manual code validation were done by **Zixuan Zhang**.

---

