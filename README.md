# AI-Powered Intelligent Trade Settlement Failure Prediction

**UC Berkeley Professional Certificate in Machine Learning & Artificial Intelligence**  
**Module 20.1 — Initial Report and Exploratory Data Analysis (EDA)**

**Author:** Rama Nuti  
**GitHub profile:** https://github.com/ramakrishna580/  
**GitHub repository:** Use your Module 20 EDA repository URL when published; https://github.com/ramakrishna580/ is the author profile

## Executive summary

This module explores whether **information available before settlement** can identify trades at elevated risk of settlement failure. The submission consists of an exploratory Jupyter notebook and this README. The notebook performs a data quality audit, descriptive visualizations, outlier assessment, leakage-conscious feature engineering, and an initial class-weighted Logistic Regression benchmark. It uses a **synthetically generated** securities trade dataset, not real or independently collected market transactions.

## Rationale

Failed settlements can increase operational work, create liquidity pressure, and potentially incur penalties. Earlier identification could help teams focus investigations on higher-risk trades. The notebook tests associations between operational friction and failures; it does not establish causation or production readiness.

## Research question and hypothesis

**Research question:** Can pre-settlement information identify trades with elevated settlement-failure risk?

**Hypothesis:** Confirmation and other processing delays, settlement-instruction changes, repeated matching attempts, inventory shortfalls, and related operational signals are associated with increased failure risk.

## Data sources

- **Input:** `data/trade_settlement_synthetic.csv`
- **Origin:** Synthetic, trade-level settlement dataset with simulated operational attributes and simulated outcomes.
- **Size:** 25,000 records × 37 original columns.
- **Coverage:** January 2, 2023 – November 28, 2025.
- **Target:** `settlement_failed` (binary).
- **Supporting dictionary:** `data/data_dictionary.csv`, if present in the project repository.
- **Important:** The CSV is not included in this module's two-file submission package. The notebook expects to be run from the project root with the CSV under `data/`.

The dataset and research focus remain consistent with the broader capstone. **For Module 20.1, the methodology is intentionally limited to EDA and an interpretable baseline**; advanced model comparisons, unsupervised learning, NLP, SHAP, and cost-based optimization belong to later capstone work.

## Methodology

1. **Load and profile** the dataset, data types, cardinalities, date coverage, missingness, and target prevalence.
2. **Audit data quality:** missing cells, duplicate rows/IDs, invalid date order, nonpositive trade amounts, invalid historical failure rates, and invalid target values.
3. **Explore distributions:** class balance over time; histograms of trade amounts, confirmation delays, matching attempts, and liquidity scores; failure rates by asset class, SSI status, and market; numeric correlations.
4. **Assess outliers:** use the 1.5×IQR rule for selected numeric fields. Flag unusual values without blanket deletion.
5. **Engineer pre-settlement features:** trade weekday/month and logarithmic transformations of size-related quantities. Exclude identifiers and outcome-derived fields, including settlement delay and failure descriptions, from predictors.
6. **Use a chronological split:** 70% train, 15% validation, 15% held-out test. Fit imputers, scalers, and categorical encoding on the training partition only.
7. **Fit an initial benchmark:** class-weighted Logistic Regression versus a dummy prior baseline. Report validation PR-AUC, ROC-AUC, precision, recall, and F1 at a 0.50 threshold. Keep the test set untouched for subsequent work.

## Results

The following numbers are **recorded outputs in the submitted Module 20.1 notebook**, not independently rerun results.

### Dataset and data quality

| Measure | Recorded result |
|---|---:|
| Records | 25,000 |
| Original columns | 37 |
| Failed settlements | 489 |
| Failure prevalence | 1.956% |
| Missing cells | 0 |
| Duplicate rows | 0 |
| Duplicate trade IDs | 0 |
| Invalid dates, amounts, historical rates, or targets in the notebook audit | 0 |
| Records retained after cleaning audit | 25,000 |
| Model predictors after feature engineering and exclusions | 31 |

The class distribution is **severely imbalanced**, so accuracy alone would be misleading. Outlier screening flagged, for example, **2,233 trade amounts (8.93%)** and **2,602 quantities (10.41%)** by the IQR criterion; these observations were not automatically discarded.

### Validation design

| Partition | Rows |
|---|---:|
| Training | 17,500 |
| Validation | 3,750 |
| Held-out test (not evaluated in this module) | 3,750 |

The recorded validation failure prevalence is **2.053%**.

### Initial model performance — validation only

| Model | PR-AUC | ROC-AUC | Precision @ 0.50 | Recall @ 0.50 | F1 @ 0.50 |
|---|---:|---:|---:|---:|---:|
| Dummy prior | 0.0205 | 0.5000 | 0.0000 | 0.0000 | 0.0000 |
| Class-weighted Logistic Regression | **0.1085** | **0.7492** | **0.0453** | **0.6104** | **0.0843** |

**Interpretation:** Logistic Regression ranks settlement failures more effectively than the dummy baseline on this synthetic validation cohort. At the illustrative 0.50 cutoff it detects about **61%** of actual failures, but only about **4.5%** of alerts are true failures. This is an important operational tradeoff: a high investigation burden may result. The 0.50 threshold has **not** been optimized, and these results do not establish deployable performance.

### Visual and exploratory outputs

The notebook contains saved charts for settlement outcome counts and monthly failure rates; distributions of trade amounts, confirmation delays, matching attempts, and liquidity; failure rates by asset class, SSI status, and market; correlation analysis; and validation precision–recall, ROC, and confusion matrix plots. These figures are exploratory and do not imply causation.

## Deliverables and repository hierarchy

This README documents the **Module 20.1 EDA repository only**, not the separate full capstone source-code repository. The following root-level files and folders are based on the author's current directory listing:

```text
Module_20_1_Initial_EDA/                 # EDA submission folder (illustrative root name)
├── data/                                # EDA input data; contents not listed
├── outputs/                             # EDA results; contents not listed
├── requirements.txt                     # Python dependencies
├── Module_20_1_Initial_EDA.ipynb        # Original EDA notebook
├── Module20_README.md                   # README in current local listing
└── Module20_EDA.ipynb                   # Additional copy of the EDA notebook
```

**Note:** `Module_20_1_Initial_EDA.ipynb` and `Module20_EDA.ipynb` are both present and have the same reported file size (483,099 bytes). For the course submission, you may keep the canonical notebook `Module_20_1_Initial_EDA.ipynb` and rename this updated `Module20_README.md` to `README.md`. Confirm the two notebooks have identical content before removing either one.

The contents of `data/` and `outputs/` were not provided in the directory listing, so their filenames are not assumed here. The broader capstone's `src/`, `notebooks/`, and `catboost_info/` folders are **not** part of this EDA-specific hierarchy.

## How to run

1. Clone or open the project repository and start Jupyter from the **repository root**.
2. Ensure `data/trade_settlement_synthetic.csv` exists.
3. Install dependencies from `requirements.txt` or install the EDA essentials:

   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn jupyter
   ```

4. Open `notebooks/Module_20_1_Initial_EDA.ipynb` and select **Run All**.
5. Inspect the data quality table, EDA figures, and validation metrics. The notebook writes `outputs/module20_validation_baseline_metrics.csv` and `outputs/module20_data_quality_audit.csv`.

## Limitations

- Data and labels are synthetic, so results may not generalize to live post-trade operations.
- Observed relationships are descriptive, not causal.
- The clean synthetic dataset does not test missing-data imputation under realistic conditions.
- Class-weighted predicted probabilities may require calibration.
- Trade date is an imperfect proxy for feature availability; production evaluation requires event timestamps and point-in-time controls.
- The held-out test set is deliberately reserved; **no final test-set claim is made in this module**.

## Next steps

For later modules, compare additional classifiers on temporal validation data, tune hyperparameters and review thresholds, evaluate calibration and alert capacity, select a model without inspecting the test labels, and evaluate the selected model once on the held-out test period. Document interpretability, drift, governance, and deployment risks.

## Submission instructions

Uploaded the updated README (named **`README.md`**) and **`Module_20_1_Initial_EDA.ipynb`**. The README results are based specifically on the Module 20.1 notebook, rather than later capstone test-set scores.

## Contact

**Rama Nuti**  
**GitHub:** https://github.com/ramakrishna580/
