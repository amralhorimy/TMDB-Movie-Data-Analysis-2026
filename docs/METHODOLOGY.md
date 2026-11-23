# Methodology

## 1. Overall approach

The notebook uses a staged workflow. It intentionally separates full-data quality and descriptive work from computationally heavier visualization and machine-learning tasks.

```text
Raw CSV
  ├── Schema and quality inspection
  ├── Cleaning and analytical field preparation
  ├── Full-data descriptive summaries
  ├── Fixed sample for selected plots
  └── Rating-supported subset for classification
        ├── Feature engineering
        ├── Train/test split
        ├── Preprocessing pipelines
        ├── Baseline and candidate models
        └── Evaluation and tuning
```

## 2. Dataset acquisition and inspection

The notebook expects a locally downloaded CSV in a `data/` directory. The recorded source file was `TMDB_movie_dataset_v11.csv`.

The executed snapshot contained 1,495,113 rows and 24 columns. Initial DataFrame memory usage was reported at approximately 1,603.91 MB, while the CSV file itself was approximately 629.12 MB.

The notebook inspects data types, uniqueness, missingness, and analytical roles before downstream processing.

## 3. Data quality and cleaning principles

The notebook documents several important controls:

- Inspect duplicate IDs before finalizing the cleaned dataset.
- Distinguish a true zero from an unavailable or unsupported measurement.
- Treat missingness as information and create observation indicators where useful.
- Parse release dates and separate future-dated records from historical temporal analysis.
- Avoid treating every zero rating or zero vote count as a valid audience rating.
- Use a rating-supported subset for rating-based modeling.
- Exclude identifiers and image/path fields from predictive features.
- Keep data-cleaning and feature transformations reproducible.

The recorded cleaned dataset contained 1,493,757 unique movie IDs.

## 4. Multiple analytical data scopes

The project uses different scopes for different computational purposes:

| Scope | Purpose |
|---|---|
| Full dataset | Core data quality, field coverage, and descriptive summaries |
| Fixed sample | Expensive plots and selected exploratory views |
| ML subset | Supervised learning on records with sufficient rating support |

The notebook describes a fixed EDA sample of 50,000 records and an ML base of 27,981 records. Sampling is used to control resource consumption, not to fabricate observations.

## 5. Exploratory and statistical analysis

The notebook includes numerical summaries and visual exploration of movie metadata, including:

- Release-year and seasonal patterns.
- Genre, language, country, and production-company distributions.
- Rating average, vote count, and popularity.
- Budget, revenue, profit, and ROI where data is available.
- Runtime and selected numerical relationships.
- Correlation and association analysis using Pearson and Spearman measures.
- Non-parametric group comparisons and chi-square tests where assumptions and group sizes permit.

Statistical tests should only be interpreted when both comparison groups are valid and non-empty. A test output containing `NaN` statistics or an empty group is not evidence for either the null or alternative hypothesis; it indicates that the test was not validly evaluated for that grouping.

## 6. Text analysis

Movie overview text is transformed using TF-IDF. The notebook also uses TruncatedSVD to project a bounded text matrix into two dimensions for exploratory visualization.

The text-only classification baseline fits TF-IDF on the training split, rather than fitting the vocabulary on the complete dataset before splitting. This helps prevent vocabulary leakage from the test set.

## 7. Feature engineering and leakage controls

The modeling workflow engineers features such as:

- Genre count.
- Overview word count.
- Tagline word count.
- Release year and month.
- Original language and status.
- Popularity, vote count, and runtime.

The notebook explicitly excludes `revenue`, `profit`, and `roi` from the high-rating classification feature set. It also excludes identifiers and path fields. A feature-level leakage audit is included.

Preprocessing is implemented within scikit-learn pipelines and fitted after the train/test split. This is important because imputation, scaling, and categorical encoding should learn their parameters from training data only.

**Interpretation caution:** the target is based on ratings, while features such as popularity and vote count can reflect post-release audience engagement. Therefore, this experiment is retrospective classification, not a pre-release prediction setup.

## 8. Reproducibility

The notebook sets `RANDOM_STATE = 42` and uses stratified train/test splitting. Exact outputs can still vary when the upstream dataset snapshot, package versions, or execution environment changes.
