# Model Card: High-Rating Classification

## 1. Model purpose

This notebook includes a supervised classification experiment that distinguishes movies meeting a high-rating threshold from those below it, among movies with sufficient rating support.

It is an educational and analytical benchmark. It is not a production recommendation system, a content-quality authority, or a pre-release box-office forecasting model.

## 2. Target definition

The notebook creates the binary target:

```python
success = (vote_average >= 7.0).astype(int)
```

The modeling base is first restricted to records that meet the notebook's `rating_support_threshold` and have a non-missing `vote_average`.

The recorded modeling base contains 27,981 rows. The positive class represents approximately 26.54% of this subset.

## 3. Features

The documented feature set contains 11 columns:

- `popularity`
- `vote_count`
- `runtime`
- `genre_count`
- `overview_word_count`
- `tagline_word_count`
- `year`
- `month`
- `original_language`
- `status`
- `adult`

The notebook excludes direct financial outcome fields (`revenue`, `profit`, and `roi`) from this target definition, along with identifiers and path fields.

The notebook flags year and month as having a medium interpretive risk because they can encode historical availability and engagement differences. Popularity and vote count are also audience-engagement signals and may be measured after release.

## 4. Split and evaluation

| Partition | Rows |
|---|---:|
| Training | 22,384 |
| Test | 5,597 |
| Total | 27,981 |

The split is 80/20, stratified by the target, with `random_state=42`.

Reported metrics are calculated on the held-out test set. The notebook also includes cross-validation and additional diagnostics.

## 5. Model comparison

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.7031 | 0.4549 | 0.6007 | 0.5177 | 0.6955 |
| Decision Tree | 0.7234 | 0.4842 | 0.6498 | 0.5549 | 0.7692 |
| Random Forest | **0.7626** | **0.5436** | **0.6552** | **0.5942** | **0.7996** |
| TF-IDF text-only | 0.6234 | 0.3600 | 0.5394 | 0.4318 | 0.6401 |

Decision Tree metrics are taken from the committed `model_metrics_detailed.csv` benchmark output.

## 6. Random Forest tuning

| Metric | Before tuning | After tuning |
|---|---:|---:|
| Accuracy | 0.762551 | 0.764695 |
| Precision | 0.543575 | 0.549354 |
| Recall | 0.655219 | 0.629630 |
| F1 | 0.594198 | 0.586759 |
| ROC-AUC | 0.799637 | 0.802667 |
| Average Precision | 0.611418 | 0.616698 |

Tuning led to small improvements in Accuracy, Precision, ROC-AUC, and Average Precision, but reduced Recall and F1 at the evaluated threshold. Consequently, the tuned model should not be described as universally better. The preferred version depends on the operational objective and decision threshold.

## 7. Limitations and risks

- **Selection bias:** the model only covers movies meeting a rating-support threshold.
- **Retrospective features:** popularity and vote count can be post-release signals. The model is not a fair simulation of what would be known before release.
- **Target subjectivity:** a 7.0 rating threshold is a project definition, not a universal definition of movie success.
- **Class imbalance:** the positive class is approximately 26.5% of the modeling subset; accuracy alone is insufficient.
- **Dataset drift:** new records and changes to existing TMDB metadata can change distributions and metrics.
- **Correlation vs causation:** feature importance and statistical associations do not establish causal effects.
- **Threshold dependence:** precision, recall, and F1 can change when the classification threshold changes.

## 8. Appropriate and inappropriate uses

**Appropriate**
- Demonstrating a reproducible tabular classification workflow.
- Comparing baseline models and evaluation metrics.
- Exploring the relationship between metadata and rating-supported labels.

**Not appropriate**
- Predicting a movie's success before release.
- Making investment, acquisition, or production decisions.
- Treating model output as an objective judgment of artistic quality.
- Generalizing performance to every movie in the raw dataset without further validation.

## 9. Artifacts

The repository includes `final_tuned_random_forest.joblib` and CSV metric reports. Load serialized scikit-learn artifacts only in a trusted environment and with compatible package versions. Pickle/joblib files can execute code during deserialization; do not load untrusted artifacts.
