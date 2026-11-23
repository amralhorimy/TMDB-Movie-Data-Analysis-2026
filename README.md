<div align="center">

# TMDB Movie Data Analysis 2026

### An End-to-End Data Analytics & Machine Learning Study of 1.49M+ Movies

[![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Notebook-Jupyter-orange?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Pandas](https://img.shields.io/badge/Data-Pandas-150458?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Scikit-learn](https://img.shields.io/badge/ML-scikit--learn-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![Status](https://img.shields.io/badge/Project-Completed-success)]()

**From raw movie metadata to data quality assessment, statistical exploration, feature engineering, and interpretable predictive modeling.**

[Notebook](TMDB_Movie_Data_Analysis_2026_FINAL.ipynb) ·
[HTML Report](TMDB_Movie_Data_Analysis_2026_FINAL.html) ·
[PDF Report](TMDB_Movie_Data_Analysis_2026_FINAL.pdf) ·
[Documentation](docs/PROJECT_OVERVIEW.md)

</div>

---

## Project at a glance

This project investigates a large-scale collection of movie metadata from The Movie Database (TMDB). It follows an end-to-end analytical workflow: understanding the raw data, assessing its quality, preparing analysis-ready datasets, exploring patterns, testing statistical relationships, engineering features, and evaluating machine-learning models.

| Project dimension | Snapshot |
|---|---:|
| Original dataset | **1,495,113 rows × 24 columns** |
| Cleaned unique movie records | **1,493,757** |
| Raw CSV size observed during analysis | **~629 MB** |
| Movie metadata coverage | Titles, dates, genres, ratings, votes, revenue, budget, runtime, languages, countries, companies, keywords, and text |
| ML modeling sample | **27,981 movies** |
| Train / test split | **80% / 20%**, stratified |
| Reproducibility seed | `42` |

> Dataset counts and measurements describe the snapshot used when this notebook was executed. The upstream Kaggle dataset may change over time; results can differ if a newer snapshot is downloaded.

## What this project explores

- **Data quality:** duplicate identifiers, missingness, zero-valued fields, date validity, and the distinction between unavailable information and genuine zero values.
- **Movie landscape:** release-year patterns, genres, language, production countries, and production companies.
- **Audience signals:** ratings, vote counts, and popularity, including the uneven level of rating support across titles.
- **Financial patterns:** budget, revenue, profit, and ROI where the required values are available.
- **Time and category analysis:** release seasonality, genre combinations, language and country distributions.
- **Statistical analysis:** Pearson and Spearman associations, non-parametric comparisons, and chi-square testing where applicable.
- **Text analysis:** TF-IDF features from movie overviews and a TruncatedSVD projection.
- **Machine learning:** a clearly defined high-rating classification task, preprocessing pipelines, model comparison, cross-validation, feature analysis, and hyperparameter tuning.

## Selected results

### Classification benchmark

The modeling task defines a *high-rated movie* as a title with `vote_average >= 7.0`, restricted to movies meeting the notebook's rating-support threshold. This is a retrospective classification experiment—not a claim that a movie's future success can be known before release.

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.7031 | 0.4549 | 0.6007 | 0.5177 | 0.6955 |
| Random Forest | **0.7626** | **0.5436** | **0.6552** | **0.5942** | **0.7996** |
| TF-IDF text-only | 0.6234 | 0.3600 | 0.5394 | 0.4318 | 0.6401 |

The Random Forest achieved the strongest untuned results among the compared models on this split.

### Hyperparameter-tuning comparison

| Metric | Before tuning | After tuning |
|---|---:|---:|
| Accuracy | 0.7626 | 0.7647 |
| Precision | 0.5436 | 0.5494 |
| Recall | 0.6552 | 0.6296 |
| F1 | **0.5942** | 0.5868 |
| ROC-AUC | 0.7996 | **0.8027** |
| Average Precision | 0.6114 | **0.6167** |

Tuning improved ROC-AUC and Average Precision slightly, but reduced F1 and Recall at the default classification threshold. This is an important result: hyperparameter tuning does not guarantee improvement across every metric. Model selection should follow the intended use case and the cost of false positives versus false negatives.

More details: [Modeling and Evaluation](docs/MODEL_CARD.md).

## Workflow

```text
Raw TMDB CSV
    │
    ▼
Dataset inspection & schema understanding
    │
    ▼
Data quality checks and cleaning
    │
    ├── Full cleaned data → descriptive statistics & data-quality analysis
    │
    ├── Fixed analysis sample → computationally heavier visual exploration
    │
    └── Rating-supported ML subset → feature engineering
                                      │
                                      ▼
                              Leakage-aware split
                                      │
                                      ▼
                         Preprocessing pipelines & models
                                      │
                                      ▼
                       Evaluation, comparison & reporting
```

The project deliberately uses different data scopes for different tasks: the full dataset for core quality checks and descriptive statistics, a fixed sample for expensive visualizations, and a smaller modeling subset to keep training practical in a CPU-only environment.

## Repository contents

| File | Purpose |
|---|---|
| `TMDB_Movie_Data_Analysis_2026_FINAL.ipynb` | Main executable notebook and full analytical workflow |
| `TMDB_Movie_Data_Analysis_2026_FINAL.html` | Browser-friendly exported notebook report |
| `TMDB_Movie_Data_Analysis_2026_FINAL.pdf` | Portable report |
| `classification_reports.csv` | Per-class and aggregate classification report metrics |
| `model_metrics_detailed.csv` | Detailed model benchmark output |
| `tuning_comparison.csv` | Random Forest before/after tuning comparison |
| `final_tuned_random_forest.joblib` | Serialized tuned Random Forest artifact |
| `requirements.txt` | Python dependencies |
| `envieroment.md` | Recorded execution environment (filename retained as currently present) |
| `docs/` | Project, methodology, reproducibility, and model documentation |

## Getting started

### 1. Clone the repository

```bash
git clone https://github.com/amralhorimy/TMDB-Movie-Data-Analysis-2026.git
cd TMDB-Movie-Data-Analysis-2026
```

### 2. Create and activate a virtual environment

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

macOS / Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Obtain the dataset

The raw dataset is not included in this repository. Download the CSV from the source dataset page:

[TMDB Movies Dataset on Kaggle](https://www.kaggle.com/datasets/asaniczka/tmdb-movies-dataset-2023-930k-movies)

Create a `data/` directory and place the downloaded CSV inside it. The notebook discovers a CSV file in that directory and expects the source file to be available locally. The dataset used in the recorded run was named `TMDB_movie_dataset_v11.csv`.

```text
project/
├── data/
│   └── TMDB_movie_dataset_v11.csv
├── TMDB_Movie_Data_Analysis_2026_FINAL.ipynb
└── ...
```

### 5. Run the notebook

```bash
jupyter notebook TMDB_Movie_Data_Analysis_2026_FINAL.ipynb
```

Run the notebook from top to bottom. The analysis uses `RANDOM_STATE = 42` for deterministic sampling and splitting where supported.

> **Resource note:** The raw CSV was approximately 629 MB, and the notebook reported roughly 1.6 GB of in-memory DataFrame usage at the initial load. Allow additional memory for intermediate tables, plotting, and model fitting. The recorded execution used Linux, Python 3.12, CPU-only processing, and approximately 10 GiB RAM.

## Documentation

- [Project overview](docs/PROJECT_OVERVIEW.md) — goals, scope, questions, and deliverables.
- [Methodology](docs/METHODOLOGY.md) — data preparation, analysis design, statistics, and feature engineering.
- [Model card](docs/MODEL_CARD.md) — target definition, model results, interpretation, and limitations.
- [Reproducibility guide](docs/REPRODUCIBILITY.md) — environment, data setup, execution, and artifact notes.
- [Data dictionary](docs/DATA_DICTIONARY.md) — the 24 source fields and their analytical roles.

## Important limitations and responsible interpretation

1. **Dataset snapshot:** TMDB metadata is continuously updated. The downloaded snapshot, not just the dataset title, determines the results.
2. **Missingness and zeros:** a zero rating, zero vote count, or zero financial value may represent missing or unsupported information rather than a meaningful measured value. Interpret each field in context.
3. **Selection bias:** the ML subset is restricted to movies with sufficient rating support. Its results should not be generalized to every record in the raw dataset.
4. **Retrospective prediction:** the classification experiment uses variables such as popularity and vote count that may be observed after release. It is not a pre-release forecasting model.
5. **Association is not causation:** correlations and feature importance do not establish causal relationships.
6. **Metric trade-offs:** tuning improved some metrics and reduced others. The tuned estimator is not automatically superior for every decision threshold or objective.
7. **Compute constraints:** large-scale visual exploration and model training use bounded samples to fit the available CPU and memory budget.

## Data source and attribution

The dataset was obtained from the [TMDB Movies Dataset on Kaggle](https://www.kaggle.com/datasets/asaniczka/tmdb-movies-dataset-2023-930k-movies). Please consult the dataset page for its current description, attribution requirements, and license/usage terms before redistributing the data.

This project is an independent analysis and is not affiliated with, endorsed by, or sponsored by TMDB or Kaggle. TMDB names and related metadata are referenced for analytical purposes.

## Author

**Amr Alhorimy**

- GitHub: [@amralhorimy](https://github.com/amralhorimy)

If you find the project useful, consider giving the repository a ⭐, sharing feedback, or opening an issue with a reproducible suggestion.

---

<div align="center">

**Explore the notebook · Review the results · Reproduce the analysis**

</div>
