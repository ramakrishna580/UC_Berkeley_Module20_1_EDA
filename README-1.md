### Project Title

**AI-Powered Intelligent Trade Settlement Failure Prediction — Module 20.1 Initial EDA**

**Author:** Rama Nuti  
**GitHub:** https://github.com/ramakrishna580/  
**Program:** UC Berkeley Professional Certificate in Machine Learning & Artificial Intelligence

#### Executive summary

This project explores whether pre-settlement operational information can help identify securities trades at risk of settlement failure. The Module 20.1 notebook examines a **synthetic** trade dataset, audits data quality, visualizes relationships, creates leakage-conscious features, and evaluates an initial class-weighted Logistic Regression benchmark. Findings are preliminary and should not be interpreted as performance on live trades.

#### Rationale

Settlement failures can cause operational rework, liquidity demands, and possible penalties. Earlier risk detection could help operations teams prioritize investigations. Because failures are rare, the analysis emphasizes precision–recall metrics rather than classification accuracy alone.

#### Research Question

**Can data available before settlement identify trades with an elevated probability of settlement failure?** The working hypothesis is that operational friction—such as confirmation delays, matching attempts, settlement-instruction issues, and inventory shortfalls—may be associated with failures. These are associations, not causal conclusions.

#### Data Sources

- **Source:** `data/trade_settlement_synthetic.csv`, a synthetically generated trade-level dataset, not an external production trading feed.
- **Observations:** 25,000 trades and 37 original columns.
- **Date coverage:** January 2, 2023 through November 28, 2025.
- **Target:** `settlement_failed` (binary); 489 failed trades, or **1.956%**.
- **Reproducibility:** The notebook expects the CSV in the `data/` directory. The directory contents beyond the notebook's named CSV have not been independently verified.

#### Methodology

1. Inspect shape, data types, date range, target balance, and quality issues (nulls, duplicates, invalid dates/values).
2. Perform EDA: outcome distribution and monthly trend, numeric distributions, failure rates by categorical attributes, correlations, and IQR-based outlier screening.
3. Engineer calendar and log-scale features using information intended to be available before settlement. Exclude identifiers and post-outcome information from model inputs.
4. Split chronologically into **70% training / 15% validation / 15% held-out test**. Fit preprocessing on training data only.
5. Compare a dummy prior classifier with class-weighted Logistic Regression on **validation** using PR-AUC, ROC-AUC, precision, recall, and F1. Reserve the test set for later capstone evaluation.

#### Results

**Results below are the saved outputs in `Module_20_1_Initial_EDA.ipynb`; the notebook was not rerun for this README update.**

| EDA / data-quality measure | Notebook result |
|---|---:|
| Trades | 25,000 |
| Original columns | 37 |
| Failed settlements | 489 |
| Failure prevalence | 1.956% |
| Missing cells | 0 |
| Duplicate records / duplicate trade IDs | 0 / 0 |
| Invalid values flagged in the notebook's core quality checks | 0 |
| Trade amount IQR outlier flags | 2,233 (8.93%) |
| Quantity IQR outlier flags | 2,602 (10.41%) |

Outliers were flagged for review, not automatically removed. The very low failure rate makes a simple accuracy score potentially misleading.

**Chronological partitions:** training 17,500 trades; validation 3,750; held-out test 3,750. Validation failure prevalence: **2.053%**.

| Validation model | PR-AUC | ROC-AUC | Precision @ 0.50 | Recall @ 0.50 | F1 @ 0.50 |
|---|---:|---:|---:|---:|---:|
| Dummy prior | 0.0205 | 0.5000 | 0.0000 | 0.0000 | 0.0000 |
| Class-weighted Logistic Regression | **0.1085** | **0.7492** | **0.0453** | **0.6104** | **0.0843** |

**Interpretation:** The Logistic Regression baseline has better ranking performance than the dummy baseline. At the unoptimized 0.50 threshold, it detects roughly 61% of failures, but only about 4.5% of its positive predictions are correct, indicating a potentially large false-alert workload. These are **synthetic-data validation results**, not held-out test results or realized business savings.

The notebook also contains visual analysis of failure prevalence over time, trade amounts, confirmation delays, matching attempts, liquidity, asset classes, SSI status, markets, correlations, and validation precision–recall / ROC curves.

#### Next steps

Compare additional classifiers and calibrated thresholds in later capstone modules, evaluate the operational alert workload, and perform a final one-time evaluation on the held-out test set. Confirm point-in-time availability of all predictors before considering real-world use. Synthetic-data performance does not establish production generalization.

#### Outline of project

Current EDA submission directory (based on the author's supplied root listing):

```text
EDA-project-root/
├── data/
├── outputs/
├── requirements.txt
├── Module_20_1_Initial_EDA.ipynb
├── Module20_README.md
└── Module20_EDA.ipynb
```

- [Module 20.1 initial EDA notebook](Module_20_1_Initial_EDA.ipynb)
- [Additional local copy of EDA notebook](Module20_EDA.ipynb)

The two notebook filenames were listed in the author's directory; they should be compared before treating them as distinct analyses. The uploaded README templates did not contain additional notebook links. For the course, upload the notebook(s) actually used and the updated README file(s) to GitHub, then submit the repository link. This README is intended for the EDA-only folder, not the larger final capstone hierarchy.

**Run instructions:** From the EDA project root, install `requirements.txt`, ensure `data/trade_settlement_synthetic.csv` is present, and open `Module_20_1_Initial_EDA.ipynb` in Jupyter. Do not use a `notebooks/` prefix: the supplied directory listing places the notebook in the root.

##### Contact and Further Information

**Rama Nuti**  
GitHub profile: https://github.com/ramakrishna580/  
Repository URL: add the exact repository link once published (the profile URL alone does not identify the project repository). No email address was supplied.
