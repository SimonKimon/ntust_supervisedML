# NTUST Supervised Machine Learning – Final Presentation

Team repository for the final project of *Supervised Machine Learning* (NTUST, fall 2026).
Team leader: Simon Klaes.

## Key facts

| | |
|---|---|
| Presentation | **23 Dec 2026, 15:30–18:20, room RB510** |
| Assigned datasets (lottery, 5 Nov) | **ID 12** Parkinsons Telemonitoring · **ID 18** Online News Popularity |
| Task | Pick **one** of the two datasets. Both are **regression** problems. |
| Models | At least **4** of: multivariable linear regression, KNN, SVM (nonlinear/Gaussian), gradient boosting, random forest. Stacking optional. Neural networks are *not* recommended by the professor. |
| Assignment sheets | [`docs/assignment/final_present.pdf`](docs/assignment/final_present.pdf), [`docs/assignment/dataset_list_final.pdf`](docs/assignment/dataset_list_final.pdf) |

The professor's advice: start early – do not leave it to the last week.

## Dataset options

Facts from the UCI pages; everything marked *(check)* is still to be confirmed with the data.

| | ID 12 – Parkinsons Telemonitoring | ID 18 – Online News Popularity |
|---|---|---|
| UCI | [dataset 189](https://archive.ics.uci.edu/dataset/189/parkinsons+telemonitoring) | [dataset 332](https://archive.ics.uci.edu/dataset/332/online+news+popularity) |
| Size | 5,875 recordings from 42 patients | 39,644 articles (CSV; the UCI page says 39,797) |
| Features | 19 (age, sex, test_time, 16 voice measures) | 58 predictive (+ `url`, `timedelta`, not predictive) |
| Target | `motor_UPDRS` or `total_UPDRS` (choose one, drop the other) | `shares` |
| Missing values | none | none |
| Watch out | ~200 recordings per patient → split by patient (`GroupKFold` on `subject#`), otherwise CV scores are inflated by leakage. Jitter/shimmer variants are strongly collinear. | Target is probably heavily skewed → consider log transform *(check)*. Gaussian SVM grid search on ~40k rows is slow. |

## Checklist (from the assignment sheet)

- [ ] All features or a selected subset? Why?
- [ ] Missing values – how were they handled?
- [ ] Multicollinearity in the linear model?
- [ ] Histograms of target and features
- [ ] Encoding and scaling
- [ ] Hyperparameter grid search for every model
- [ ] Distance metric for distance-based models
- [ ] Cross-validation
- [ ] Measures against overfitting
- [ ] Which model performed best, by which metrics (MAE, RMSE, R², adj. R²)?
- [ ] SHAP or permutation feature importance for every model – consistent across models?

(Class balance and binary/multiclass items on the sheet apply to classification only.)

## Repository layout

```
data/raw/          downloaded dataset (not committed)
data/processed/    cleaned / transformed data (not committed)
notebooks/         exploration and experiments
src/               reusable code (preprocessing, training, evaluation)
results/figures/   plots for the slides
slides/            presentation
docs/assignment/   assignment sheets from Moodle
```

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

The datasets are not committed. Download both zips from the *Final Presentation* section on
Moodle and unzip them so that you get:

```
data/raw/parkinsons_telemonitoring/parkinsons_updrs.data    # CSV with header, 5,875 rows x 22 columns
data/raw/parkinsons_telemonitoring/parkinsons_updrs.names   # description
data/raw/online_news_popularity/OnlineNewsPopularity.csv    # 39,644 rows x 61 columns
data/raw/online_news_popularity/OnlineNewsPopularity.names  # description
```

```python
import pandas as pd
df = pd.read_csv("data/raw/parkinsons_telemonitoring/parkinsons_updrs.data")
# Online News: column names in the CSV start with a space -> skipinitialspace=True
news = pd.read_csv("data/raw/online_news_popularity/OnlineNewsPopularity.csv", skipinitialspace=True)
```

## Working together

- One branch per task (for example `eda`, `model-svm`), merge into `main` when it runs.
- Do not commit data files, virtual environments or notebook checkpoints (see `.gitignore`).
- Homework 1–3 are individual work and do not belong in this repository.
