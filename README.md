# Cybersecurity Intrusion Detection Dashboard

An interactive analytics dashboard for exploring a cybersecurity intrusion detection dataset. Built with [Plotly Dash](https://dash.plotly.com/) and [Polars](https://pola.rs/), it visualises attack patterns across protocols, browsers, login behaviour, session features, and feature correlations.

---

## Project structure

```
.
├── __main__.py              # Entry point → runs the dashboard
├── dashboard/
│   └── run.py               # Dash app: figures, layout, stats
└── datasets/
    ├── __main__.py          # Entry point → dataset CLI
    ├── download.py          # Kaggle download + registry logic
    └── datasets.json        # Registry of known datasets
```

Data is cached under `cache/` (created automatically on first download):

```
cache/
└── cybersecurity-intrusion-detection-datatset/
    └── cybersecurity_intrusion_data.csv
```

---

## Requirements

- Python 3.10+
- A [Kaggle API token](https://www.kaggle.com/settings/account) (`~/.kaggle/kaggle.json`)

Install dependencies (example with pip):

```bash
pip install dash plotly polars numpy kagglehub
```

---

## Quickstart

### 1. Download the dataset

```bash
python -m datasets download cybersecurity-intrusion-detection-datatset
```

This pulls the dataset from Kaggle via `kagglehub` and places it in `cache/cybersecurity-intrusion-detection-datatset/`.

### 2. Run the dashboard

```bash
python -m dashboard
```

Then open your browser at **http://127.0.0.1:8050**.

---

## Dataset CLI reference

The `datasets` module is a small CLI for managing Kaggle datasets.

```bash
# List all registered datasets and their download status
python -m datasets list

# Download a specific dataset
python -m datasets download <dataset-key>

# Force re-download (overwrite existing cache)
python -m datasets download <dataset-key> --force

# Download all registered datasets
python -m datasets download --all

# Register a new Kaggle dataset
python -m datasets add <dataset-key> <kaggle-handle>
# e.g.
python -m datasets add my-dataset owner/dataset-name
```

Dataset metadata is stored in `datasets/datasets.json`. The currently registered dataset is:

| Key | Kaggle handle |
|-----|---------------|
| `cybersecurity-intrusion-detection-datatset` | `dnkumars/cybersecurity-intrusion-detection-dataset` |

---

## Dashboard sections

| Section | Charts |
|---------|--------|
| **Overview** | Overall attack rate (donut), attack rate by protocol, attack rate by encryption |
| **Attack rate by category** | Attack rate by browser, failed logins vs attack share, unusual time access |
| **Distributions** | Login attempts box plot, IP reputation violin, session duration histogram, packet size box plot |
| **Feature correlation** | Pearson correlation heatmap across all numeric features |

Summary stat cards at the top show total sessions, attacks detected, average IP reputation scores, and average session duration.

---

## Configuration

The dataset path and colour scheme are defined at the top of `dashboard/run.py`:

```python
DATA_PATH = "cache/cybersecurity-intrusion-detection-datatset/cybersecurity_intrusion_data.csv"

ATTACK_COLOR = "#D85A30"
SAFE_COLOR   = "#1D9E75"
```

Adjust `DATA_PATH` if you store data elsewhere, or swap the hex values to retheme the charts.
