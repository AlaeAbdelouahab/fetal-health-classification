# Fetal Health Classification Using Machine Learning

A comparative machine learning study of fetal health classification using cardiotocography (CTG) measurements. The project explores data preparation, supervised classification, model evaluation, and error analysis across three fetal health categories: **Normal**, **Suspect**, and **Pathological**.


## Project objectives

- Explore the CTG dataset and examine class distribution and data quality.
- Prepare a reproducible train/test dataset while avoiding scaling-related data leakage.
- Train and evaluate **Support Vector Machine (SVM)**, **Random Forest**, and **XGBoost** classifiers.
- Compare overall and per-class performance, with particular attention to minority classes and classification errors.
- Examine the effects of hyperparameter tuning and potential overfitting.

## Workflow

```text
data/fetal_health.csv
        |
        v
ETL_FetalHealth.ipynb
  - Exploratory data analysis
  - Duplicate removal
  - Stratified train/test split
  - Target label encoding
  - StandardScaler fitted on training data
        |
        v
prepared_fetal_data.pkl
        |
        +--> SVM_FetalHealth.ipynb
        +--> RandomForest_FetalHealth.ipynb
        +--> XGBoost_FetalHealth.ipynb
        |
        v
Comparaison_Modeles.ipynb
  - Model comparison
  - Per-class metrics and confusion matrices
  - ROC analysis and train/test performance
```

## Repository structure

| File | Purpose |
| --- | --- |
| `ETL_FetalHealth.ipynb` | Data inspection, exploratory analysis, preprocessing, and serialization of prepared data. |
| `SVM_FetalHealth.ipynb` | SVM modeling, tuning, and evaluation. |
| `RandomForest_FetalHealth.ipynb` | Random Forest modeling, tuning, and evaluation. |
| `XGBoost_FetalHealth.ipynb` | XGBoost modeling, class-imbalance handling, tuning, and evaluation. |
| `Comparaison_Modeles.ipynb` | Side-by-side evaluation of the three classifiers. |
| `data/fetal_health.csv` | Input CTG dataset (include only if redistribution is permitted). |
| `requirements.txt` | Python dependencies. |

The preprocessing notebook creates `prepared_fetal_data.pkl`. The XGBoost notebook also saves a baseline model artifact, `xgboost_fetal_health_baseline.pkl`. These generated files can be recreated by running the relevant notebooks.

## Methodology

### 1. Data exploration and preparation

The ETL notebook examines the distribution of the three target classes, checks missing values and duplicate records, and visualizes selected CTG features. It removes duplicates, performs a stratified train/test split, encodes class labels from `{1, 2, 3}` to `{0, 1, 2}`, and standardizes features using a scaler fitted on the training set only.

### 2. Model development

Three classifiers are studied independently:

- **SVM:** Support vector classification with hyperparameter search.
- **Random Forest:** Ensemble classification with baseline and tuned configurations.
- **XGBoost:** Gradient-boosted trees, including sample weighting to address class imbalance and grid-search evaluation.

### 3. Evaluation

The notebooks use accuracy, precision, recall, F1-score, classification reports, confusion matrices, and multiclass ROC/AUC analysis. The comparison notebook additionally examines model training times and train/test performance gaps.

## Selected results

In the **XGBoost notebook**, the **baseline model** achieved the following results on the held-out test set:

| Metric | XGBoost baseline |
| --- | ---: |
| Accuracy | **96.45%** |
| Macro F1-score | **94.24%** |
| Pathological-class recall | **100%** |

These are experimental results on this dataset and split, **not clinical performance guarantees**. The notebooks also show that hyperparameter tuning did not necessarily improve held-out test performance: the tuned XGBoost configuration scored lower than the baseline in that evaluation. The **Suspect** class remained an important source of errors.

For the full SVM/Random Forest/XGBoost comparison, see [`Comparaison_Modeles.ipynb`](Comparaison_Modeles.ipynb).

## Getting started

### Prerequisites

- Python and Jupyter Notebook or VS Code with Jupyter support.
- The CTG dataset at `data/fetal_health.csv`.

### Installation

```bash
python -m venv .venv
```

On Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

On macOS/Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install -r requirements.txt
```

### Execution order

1. Put the dataset at `data/fetal_health.csv`.
2. Run **`ETL_FetalHealth.ipynb`** to generate `prepared_fetal_data.pkl`.
3. Run **`SVM_FetalHealth.ipynb`**, **`RandomForest_FetalHealth.ipynb`**, and **`XGBoost_FetalHealth.ipynb`** to explore individual models.
4. Run **`Comparaison_Modeles.ipynb`** for the consolidated comparison.

Run notebooks from the repository root so their relative file paths resolve correctly. Some training and hyperparameter-search cells may take time to complete.

## Dataset

The project uses a fetal health classification dataset based on cardiotocography (CTG) features, with three labeled classes. The notebooks expect a CSV named `fetal_health.csv` inside `data/`.

**Before publishing:** add the dataset's original source URL, attribution, and license/redistribution terms here. If redistribution is not allowed or unclear, do not upload the CSV; provide acquisition instructions instead.

## Limitations

- Class imbalance makes performance on the **Suspect** and **Pathological** classes particularly important.
- Results depend on the available dataset, evaluation split, and model settings.
- High test accuracy does not establish external generalizability or clinical suitability.
- A full clinical assessment would require additional datasets and domain-specific validation.
