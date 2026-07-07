#  Advanced Machine Learning  House Price Prediction Model Comparison

> Week 5 of the AnalystLab Africa Data Science Internship (Batch B)  comparing baseline, tree-based, and ensemble regression models with hyperparameter tuning on the Housing Prices dataset.

---

## 📋 Project Overview

This project builds and compares six regression models — a Linear Regression baseline, Decision Tree, Random Forest, Gradient Boosting, and tuned versions of both ensemble models using GridSearchCV. The goal is to identify the best-performing model and, more importantly, to understand why certain models outperform others — including an honest finding that hyperparameter tuning did not always improve results.

---

## 📁 Repository Structure

```
week5-advanced-ml/
│
├── Housing_Cleaned.csv                          # Cleaned dataset (from Week 1)
├── Week5_Advanced_ML.ipynb                      # Full notebook: all 6 steps
├── Performance_Comparison_Report.docx     # Performance comparison report
└── README.md                                    # This file
```

**Dataset link:** [Housing Prices Dataset (Kaggle)](https://www.kaggle.com/datasets/yasserh/housing-prices-dataset)

---

## 🎯 Objective

Improve predictive performance using advanced machine learning algorithms and optimization techniques, compare multiple models systematically, and identify the best-performing approach — with a clear explanation of why it performed best.

---

## 📊 Dataset

| Property | Details |
|----------|---------|
| Source | Housing_Cleaned.csv (Week 1 output) |
| Records | 545 rows |
| Features used | 12 |
| Target variable | `price` (continuous, £) |
| Missing values | 0 |
| Train / Test split | 436 / 109 rows (80/20, random_state=42) |

**Features:** area, bedrooms, bathrooms, stories, parking, mainroad, guestroom, basement, hotwaterheating, airconditioning, prefarea, furnishingstatus_enc

---

## 🔬 Methodology

### Step 1 — Data Preparation
- Loaded `Housing_Cleaned.csv`, verified zero missing values and zero duplicates
- Confirmed all categorical columns were encoded during Week 1
- Applied `StandardScaler` for Linear Regression only — tree-based models used unscaled features

### Step 2 — Baseline Model
- Trained **Linear Regression** as the reference point for all comparisons

### Step 3 — Advanced Models
- **Decision Tree Regressor** — default settings
- **Random Forest Regressor** — 100 estimators, default settings
- **Gradient Boosting Regressor** — default settings

### Step 4 — Hyperparameter Tuning
- **GridSearchCV** with 5-fold cross-validation applied to both Random Forest and Gradient Boosting
- Parameters tuned: `n_estimators`, `max_depth`, `min_samples_split`, `learning_rate` (GB only)

### Step 5 — Evaluation
- MAE, MSE, RMSE, and R² computed for every model on the held-out test set

### Step 6 — Comparison
- Full performance table built and sorted by RMSE; best model identified and explained

---

## 📈 Results — Performance Comparison

| Model | MAE | RMSE | R² |
|-------|-----|------|-----|
| 🏆 **Gradient Boosting** | £963,191 | **£1,300,896** | **0.6652** |
| Linear Regression (Baseline) | £979,680 | £1,331,071 | 0.6495 |
| Gradient Boosting (Tuned) | £1,002,168 | £1,358,914 | 0.6347 |
| Random Forest | £1,024,081 | £1,398,901 | 0.6128 |
| Random Forest (Tuned) | £1,045,274 | £1,438,459 | 0.5906 |
| Decision Tree | £1,270,789 | £1,718,102 | 0.4160 |

> Sorted by RMSE — lower is better

---

## 🏆 Best Model: Gradient Boosting (Default Parameters)

**Why it performed best:**
- Builds trees sequentially, with each new tree correcting the residual errors of the previous ones — capturing non-linear pricing patterns more precisely than a single linear equation
- More sample-efficient than Random Forest on this small dataset (545 rows)
- Avoids the severe overfitting of a single Decision Tree (R²=0.42) by combining many shallow trees

---

## 💡 Key Finding: Hyperparameter Tuning Made Results Worse

Both tuned models underperformed their default versions on the held-out test set:

| Model | Default R² | Tuned R² | Change |
|-------|-----------|----------|--------|
| Random Forest | 0.6128 | 0.5906 | ↓ 0.022 |
| Gradient Boosting | 0.6652 | 0.6347 | ↓ 0.031 |

**Why:** With only 545 total rows, each 5-fold CV fold contains roughly 87–109 samples — small enough that cross-validation scores carry significant noise. GridSearchCV found parameters that performed well within those noisy folds but did not generalize to the real test set. This is a common pitfall on small datasets, and an important lesson: tuning results must always be validated against true held-out performance, not assumed from CV score alone.

---

## 🔑 Key Insights

1. **Gradient Boosting (default) is the best model** — RMSE £1,300,896, R² 0.6652
2. **Linear Regression is a highly competitive baseline** — R² 0.6495, only 1.6 points behind the best model despite far less complexity
3. **Ensembling substantially outperforms a single Decision Tree** — Random Forest and Gradient Boosting both improve R² by 17–22 points over a lone tree
4. **Hyperparameter tuning is not automatically beneficial** — especially on small datasets; always validate against a true held-out test set
5. **Feature importance is consistent across all three tree-based models** — Area and Bathrooms rank as the top two price drivers in Decision Tree, Random Forest, and Gradient Boosting alike, confirming these are robust signals rather than model-specific artifacts

---

## 🛠️ Technologies Used

- **Python 3.x**
- **Pandas / NumPy** — data manipulation
- **Scikit-learn** — LinearRegression, DecisionTreeRegressor, RandomForestRegressor, GradientBoostingRegressor, GridSearchCV, StandardScaler, metrics
- **Matplotlib / Seaborn** — visualizations
- **Jupyter Notebook** — interactive analysis environment

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter openpyxl
```

### Run the Notebook

```bash
git clone https://github.com/YOUR_USERNAME/week5-advanced-ml.git
cd week5-advanced-ml
jupyter notebook Week5_Advanced_ML.ipynb
```

---

## 📄 Deliverables

| File | Description |
|------|-------------|
| `Week5_Advanced_ML.ipynb` | Full 6-step pipeline: preprocessing, baseline, 3 advanced models, GridSearchCV tuning, evaluation, visualizations, conclusions |
| `Week5_Performance_Comparison_Report.docx` | Report covering problem statement, dataset, models, metrics, comparison table, key findings, recommendations |
| `w5_fig1_model_comparison.png` | MAE / RMSE / R² bar charts + best model Actual vs Predicted scatter |
| `w5_fig2_feature_importance.png` | Feature importance side by side: Decision Tree, Random Forest, Gradient Boosting |
| `w5_fig3_tuning_comparison.png` | Before vs after tuning (RMSE and R²), residuals, all models ranked by RMSE |

---

## 📝 License

Open source under the [MIT License](LICENSE).

---

*AnalystLab Africa Data Science Internship — Batch B (June–August)*
