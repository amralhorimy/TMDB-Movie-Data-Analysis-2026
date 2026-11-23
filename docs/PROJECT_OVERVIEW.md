# Project Overview

## 1. Summary

**TMDB Movie Data Analysis 2026** is an end-to-end data analytics and machine-learning project built around a large TMDB movie metadata snapshot. Its purpose is to demonstrate a complete analytical workflow rather than a single chart or isolated model.

The notebook combines data inspection, data-quality assessment, descriptive and exploratory analysis, statistical testing, text feature extraction, feature engineering, supervised classification, evaluation, and reporting.

## 2. Objectives

- Understand the structure, coverage, and limitations of a large movie metadata dataset.
- Assess duplicates, missing values, zero-valued fields, and temporal validity.
- Explore movie production and audience-related patterns across time, genres, languages, and countries.
- Examine relationships among ratings, votes, popularity, runtime, budget, revenue, profit, and ROI where available.
- Apply statistical tests to selected analytical questions.
- Build a reproducible classification benchmark with leakage-aware preprocessing.
- Communicate results through a notebook, HTML export, PDF export, and machine-readable CSV outputs.

## 3. Dataset snapshot

The executed notebook reports:

| Item | Value |
|---|---:|
| Raw rows | 1,495,113 |
| Columns | 24 |
| Cleaned unique movie IDs | 1,493,757 |
| Raw CSV size | ~629.12 MB |
| ML modeling base | 27,981 rows |
| ML training rows | 22,384 |
| ML test rows | 5,597 |

The source file used in the recorded run was `TMDB_movie_dataset_v11.csv`, obtained from the public Kaggle dataset page. Dataset versions may change, so these figures are snapshot-specific.

## 4. Analytical questions

The project is structured to investigate questions such as:

- How complete and reliable are the available movie records?
- How has the volume of movie releases changed over time?
- Which genres and original languages occur most frequently?
- How do rating averages relate to the number of votes and popularity?
- What patterns appear in budget, revenue, profit, and ROI records?
- Are there seasonal patterns in movie releases?
- Which numerical associations are strongest, and are they linear or monotonic?
- Can selected movie metadata distinguish high-rated movies within a rating-supported subset?
- How does a text-only TF-IDF baseline compare with tabular feature models?

These are descriptive and predictive questions. They should not be interpreted as causal claims.

## 5. Deliverables

- Main Jupyter notebook containing the workflow and executed outputs.
- HTML and PDF notebook exports.
- CSV model reports and tuning comparisons.
- Serialized tuned Random Forest artifact.
- Project documentation describing methodology, results, and reproducibility.

## 6. Intended audience

- Data analysts and data-science learners interested in end-to-end projects.
- Hiring managers reviewing practical analytical workflow and communication.
- Practitioners interested in large tabular datasets and applied classification.
- Readers interested in film-industry metadata and audience signals.

## 7. Scope boundaries

This repository is an analytical study. It does not provide a production API, a live movie recommendation service, a continuously updated data pipeline, or a causal model of box-office success. The trained classifier is a retrospective benchmark under the target and feature definitions documented in the model card.
