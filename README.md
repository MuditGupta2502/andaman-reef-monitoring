# Andaman Reef Monitoring — Satellite-Driven Coral Reef Health Pipeline

An end-to-end machine learning and climate analysis system for monitoring coral reef
health in the **Andaman & Nicobar Islands (ANI)**, India, using 20 years (2004–2024)
of NASA MODIS Aqua satellite imagery and ECMWF ERA5 reanalysis data.

The project is structured as three research tasks, each documented in a Jupyter
notebook and compiled into a full research report (51 pages, PDF).

---

## Research Tasks

### Task 1 — Weekly Water Quality Prediction
Predict next-week chlorophyll-*a* concentration (a proxy for algal growth and water
clarity) at four ANI reef sites using XGBoost and Random Forest models.

- **Sites:** Havelock, Neil, Port Blair, Wandoor
- **Features:** chlorophyll lag, turbidity (K_d 490 proxy), SST, wave energy (ERA5), precipitation, seasonal encoding
- **Key finding:** Forecast skill 35–61% above a naive persistence baseline at oceanic
  reefs; Wandoor (estuarine) requires terrestrial hydrological inputs unavailable from satellite

### Task 2 — Coral Bleaching Early Warning Index
Apply the NOAA Coral Reef Watch Degree Heating Weeks (DHW) methodology to the
20-year ANI SST record to build a site-specific bleaching early warning system.

- **Key finding:** ANI thermal stress is driven by the **Indian Ocean Dipole (IOD)**,
  not El Niño (ENSO). The 2010 IOD event is the dominant bleaching event (peak
  DHW ≈ 7.95 °C-weeks). An XGBoost model trained on 2004–2009 data correctly
  forecasts the rising stress trajectory 3–4 weeks before threshold crossing in
  both 2010 and 2024 (R² = 0.59–0.72)

### Task 3 — Global Climate Stress Comparison
Apply the same DHW pipeline to six globally distributed reef systems (Florida/USA,
Great Barrier Reef/Australia, Hawaii/USA, Mediterranean/Europe, Red Sea/Middle East,
Seychelles/Africa) and compare against ANI.

- **Key finding:** Every site shows higher bleaching-stress frequency in 2014–2024
  vs 2004–2013. ANI shows the smallest intensification (+0.5 pp) of any site — but
  a statistically significant warming trend of +0.17 °C/decade means this advantage
  will narrow without emissions reductions

---

## Repository Structure

```
andaman-reef-monitoring/
├── data/
│   └── raw/                         # Input CSV datasets (see below)
├── notebooks/
│   ├── 01_andaman_eda.ipynb          # Exploratory data analysis
│   ├── 03_task1_wq_prediction.ipynb  # Task 1: water quality ML models
│   ├── 04_task2_bleaching_ews.ipynb  # Task 2: DHW bleaching early warning
│   └── 05_task3_global_climate.ipynb # Task 3: global climate comparison
├── outputs/
│   ├── task1/                        # Figures and CSVs from Task 1
│   ├── task2/                        # Figures and CSVs from Task 2
│   └── task3/                        # Figures and CSVs from Task 3
├── report/
│   └── ANI_Coral_Reef_Report.tex     # Full LaTeX research report (51 pages)
├── requirements.txt
└── README.md
```

---

## Datasets

Place the following CSV files in `data/raw/` before running the notebooks:

| File | Description | Rows | Download |
|------|-------------|------|----------|
| `Andaman_WQ_20Year_Dataset.csv` | 4 ANI sites, weekly, 2004–2024, chlor_a + SST + precip | 4,380 | [Download](https://drive.google.com/file/d/1xGrP9394_Lg2NX7RWA_SYin6TfJhS7oI/view?usp=sharing) |
| `Andaman_WQ_Harmonized_20Year.csv` | Same + Rrs_443/490, ERA5 winds, MSLP | 4,380 | [Download](https://drive.google.com/file/d/1NuzkKS2m6Ty6NRpRbLWFab7rH2xpjtYv/view?usp=sharing) |
| `Global_WQ_20Year_Dataset.csv` | 6 global reef sites, monthly, 2004–2024 | ~1,506 | [Download](https://drive.google.com/file/d/1Nw-FA1BSg3Y1cLlm8afrZNV2TLAJqshW/view?usp=sharing) |
| `Global_WQ_Harmonized_20Year.csv` | Same + Rrs bands and ERA5 fields | ~1,506 | [Download](https://drive.google.com/file/d/1f3x0m_zkJoSmg4C7MP69TF5LIfXdaTs1/view?usp=sharing) |

**Data source:** Google Earth Engine — COPERNICUS/MARINE/SATELLITE_OCEAN_COLOR/V6
(MODIS Aqua) for ocean colour; ECMWF ERA5 for atmospheric variables.

---

## Setup

Requires **Python 3.10+**.

```powershell
# Create and activate virtual environment (Windows)
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt
```

---

## Running the Notebooks

Open each notebook in Jupyter Lab or VS Code and run all cells in order.
Each notebook is self-contained — it reads from `data/raw/` and writes
figures/CSVs to `outputs/<taskN>/`.

```powershell
jupyter lab
```

Notebooks must be run in order (01 → 03 → 04 → 05) as later notebooks
depend on variables set by earlier ones only when run in the same kernel session.
Each notebook can also be run independently from a cold start.

---

## Key Results Summary

| Task | Best Model | Key Metric | Highlight |
|------|-----------|------------|-----------|
| Task 1 | XGBoost (log-space) | R² = 0.23–0.25 per oceanic site | Turbidity (K_d 490) ranks #2 predictor after chlorophyll lag |
| Task 2 | XGBoost hindcast | R² = 0.59–0.72 on 2010 IOD event | 3–4 week advance warning before DHW threshold crossing |
| Task 3 | OLS trend + DHW comparison | +0.17 °C/decade at ANI (p=0.030) | ANI has smallest DHW intensification of 7 sites studied |

---

## Report

The full research report is in `report/ANI_Coral_Reef_Report.tex` (LaTeX source).
To compile to PDF (requires TeX Live or MiKTeX):

```powershell
cd report
pdflatex ANI_Coral_Reef_Report.tex
pdflatex ANI_Coral_Reef_Report.tex   # second pass for cross-references
```

---

## Requirements

Key packages (see `requirements.txt` for pinned versions):

- `pandas`, `numpy` — data manipulation
- `scikit-learn`, `xgboost` — machine learning
- `shap` — model explainability (SHAP values)
- `matplotlib`, `seaborn` — visualisation
- `scipy`, `statsmodels` — statistical tests (OLS, Mann-Kendall)
- `jupyter` — notebook environment

---

## Citation / Academic Context

This project was completed as an **Independent Study** (March 2026).
All satellite data is freely available through Google Earth Engine and NOAA.

