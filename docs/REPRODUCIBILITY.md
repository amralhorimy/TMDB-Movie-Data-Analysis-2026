# Reproducibility Guide

## 1. Recorded environment

The repository's environment note records the following execution setup:

| Component | Recorded value |
|---|---|
| OS | Ubuntu 24.04.4 LTS |
| Architecture | x86_64 |
| CPU | Intel Xeon @ 2.50 GHz |
| Logical CPUs | 6 vCPU |
| RAM | ~10 GiB |
| Python | 3.12.3 |
| GPU | Not available |
| CUDA | Not used |
| Random seed | 42 |

The notebook output also reports pandas 3.0.5, NumPy 2.5.1, and scikit-learn 1.9.1 for that execution. See the repository's environment note and `requirements.txt` for the recorded package setup.

## 2. Data requirements

The raw CSV is not stored in the repository. Download the dataset from the [Kaggle source page](https://www.kaggle.com/datasets/asaniczka/tmdb-movies-dataset-2023-930k-movies).

Create a local `data/` directory and place the downloaded CSV there. The executed notebook used a file named `TMDB_movie_dataset_v11.csv` and searches for a CSV in the data directory.

Do not assume a newer download will produce exactly the same row counts or metrics. Record the download date and, ideally, a checksum when reproducing the experiment.

## 3. Setup

```bash
git clone https://github.com/amralhorimy/TMDB-Movie-Data-Analysis-2026.git
cd TMDB-Movie-Data-Analysis-2026

python -m venv .venv
```

Activate the environment:

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter notebook TMDB_Movie_Data_Analysis_2026_FINAL.ipynb
```

Run all cells in order.

## 4. Resource expectations

The recorded CSV file was approximately 629 MB. The initial DataFrame memory report was approximately 1.6 GB. Peak memory usage can be substantially higher because of copies, intermediate tables, one-hot encoding, sparse text matrices, plots, and model fitting.

The recorded environment was CPU-only. The notebook uses bounded samples for computationally heavier EDA and model training. A machine with more RAM may improve execution comfort, but GPU is not required for the included classical scikit-learn models.

## 5. Reproducibility checklist

- [ ] Record the exact Kaggle dataset version or download date.
- [ ] Place the CSV in `data/`.
- [ ] Install the dependencies from `requirements.txt`.
- [ ] Run the notebook from the first cell to the last.
- [ ] Confirm the reported raw and cleaned row counts.
- [ ] Confirm that train/test sizes and class proportions match the expected snapshot.
- [ ] Re-export HTML/PDF reports after a fresh run if outputs have changed.
- [ ] Keep model artifacts and metric CSVs synchronized with the notebook execution.

## 6. Artifact compatibility and safety

`final_tuned_random_forest.joblib` is a serialized Python object. Joblib/pickle deserialization should only be performed for artifacts from a trusted source. Compatible scikit-learn and dependency versions are recommended; serialized estimator compatibility across versions is not guaranteed.
