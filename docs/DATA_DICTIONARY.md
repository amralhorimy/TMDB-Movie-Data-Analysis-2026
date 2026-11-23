# Data Dictionary

## Source schema

The notebook reports 24 columns in the source movie dataset. The descriptions below summarize their common TMDB/Kaggle meaning; exact nullability and values depend on the downloaded dataset snapshot.

| Field | Typical meaning | Analytical role / notes |
|---|---|---|
| `id` | TMDB movie identifier | Record identity; exclude from model features |
| `title` | Movie title | Descriptive identifier |
| `vote_average` | Average user rating | Rating analysis and classification target source; interpret with vote support |
| `vote_count` | Number of votes | Audience support signal; zero may indicate no rating support |
| `status` | Release/production status | Categorical metadata |
| `release_date` | Release date | Temporal analysis; validate and parse |
| `revenue` | Reported revenue | Financial analysis; missing/zero values require care |
| `runtime` | Runtime in minutes | Numerical analysis and engineered features |
| `adult` | Adult-content flag | Boolean/categorical metadata |
| `backdrop_path` | TMDB backdrop image path | Media reference; not a modeling feature |
| `budget` | Reported production budget | Financial analysis; missing/zero values require care |
| `homepage` | Official homepage URL | URL metadata; not a modeling feature |
| `imdb_id` | IMDb identifier | External identifier; exclude from model features |
| `original_language` | Original language code | Categorical analysis and model feature |
| `original_title` | Original-language title | Text metadata |
| `overview` | Movie synopsis | Text analysis and TF-IDF |
| `popularity` | TMDB popularity measure | Audience/engagement signal; time-dependent |
| `poster_path` | TMDB poster image path | Media reference; not a modeling feature |
| `tagline` | Promotional tagline | Text metadata; word-count feature |
| `genres` | Genre labels | Multi-label categorical field; genre counts and distributions |
| `production_companies` | Production companies | Multi-label categorical metadata |
| `production_countries` | Production countries | Multi-label categorical metadata |
| `spoken_languages` | Spoken languages | Multi-label categorical metadata |
| `keywords` | Associated keywords | Multi-label/text metadata |

## Derived fields

The notebook also derives analytical features and measures, including:

- `year`, `month`, and movie age from release dates.
- `profit` and `roi` where budget and revenue permit meaningful calculation.
- `genre_count`, `overview_word_count`, and `tagline_word_count`.
- Genre, year, language, country, and financial summary tables.
- The binary `success` target for the rating-supported classification experiment.

## Data interpretation warnings

- A missing value is not necessarily equivalent to zero.
- Zero votes provide no meaningful rating support; a zero average should not automatically be interpreted as a genuine audience rating.
- Financial fields may be incomplete or inconsistently reported.
- Multi-label columns often contain comma-separated values; counting raw strings is not the same as counting individual labels.
- Popularity and vote counts can change over time.
- The Kaggle dataset's schema and field availability may change between versions.
