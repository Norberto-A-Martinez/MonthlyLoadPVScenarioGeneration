# Monthly Load & PV Scenario Generation

This script uses **K-Means Clustering** to reduce a full year (365 days) of hourly power load demand (load) and solar photovoltaic (PV) generation data into a set of representative daily profiles (scenarios) for each month.
The output is designed to be an input to AMPL optimization models. Your can check [here](https://plugins.ampl.com/amplcsv.html) how to do it.

## Overview

In power system optimization, using a full year of data can be computationally expensive. This script simplifies the data by:
1.  **Preprocessing:** Converting 15-minute load data into hourly totals and reshaping PV data into daily blocks.
2.  **Normalization:** Scaling the load curves relative to the annual peak.
3.  **Clustering:** Running K-Means on each month to find typical "shapes" for the day.
4.  **Probability Calculation:** Determining how likely each representative day is to occur.

---

## Prerequisites

You need Python installed along with the following libraries:

```bash
pip install numpy pandas scikit-learn
```

## Folder Structure

The script looks for data in a folder named `input-data/`. Ensure your files are named correctly:

```text
├── main_script.py
└── input-data/
    ├── ninja_pv_*.csv   # Solar data (must contain an 'electricity' column)
    └── load_clean.csv   # Load data (must contain 'E1A_AZI_A' column, separated by ';')
```

## Output

The script exports four CSV files to the `input-data/` folder, which are formatted for easy import into optimization models (like AMPL):

| File | Columns | Description |
| --- | --- | --- |
| `parametros_load_curvas.csv` | M, D, T, curve_load | Hourly load values for each scenario (D) per month (M). |
| `parametros_pv_curvas.csv` | M, S, T, curve_pv | Hourly PV values for each scenario (S) per month (M). |
| `parametros_load_probs.csv` | M, D, prob_D | Probability of each load scenario. |
| `parametros_pv_probs.csv` | M, S, prob_S | Probability of each PV scenario. |
