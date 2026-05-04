# Offshore Wind Farm Harmonic Dataset

A multi-node harmonic-state dataset for offshore wind farms, designed for temporal harmonic reconstruction, cross-node virtual sensing, and physics-guided learning research.

---

## Overview

This dataset contains time-synchronized wind-field and electrical measurements together with total harmonic distortion (THD) labels at two key nodes of a typical offshore wind-farm collector system:

- **B60 (PCC side):** 60 kV point of common coupling, closer to the grid.
- **B575 (WT side):** 575 V turbine terminal, closer to the converter.

The data are organized chronologically and can be used directly for sequence-modeling experiments (e.g., sliding-window state reconstruction).

> **Data nature:** This is a **synthetic/physics-informed dataset** generated to emulate the harmonic behavior of a DFIG-based offshore wind farm with a representative collector topology (36 km submarine cable, step-up transformers, etc.). It is intended for reproducible academic research and method benchmarking.

---

## File Description

| File                           | Size    | Rows   | Columns | Format          |
| ------------------------------ | ------- | ------ | ------- | --------------- |
| `synthetic_wind_farm_data.csv` | ~2.1 MB | 11,724 | 15      | CSV with header |

- **Temporal coverage:** 2024-06-01 00:00:00 – 2025-12-03 23:00:00
- **Base sampling:** Hourly (irregular gaps exist; see Notes below)
- **Ordering:** Chronological (do **not** shuffle for train/validation/test splits)

---

## Column Description

### Time stamps

| Column     | Type     | Description                             |
| ---------- | -------- | --------------------------------------- |
| `datetime` | ISO 8601 | Primary timestamp (YYYY-MM-DD HH:MM:SS) |
| `YEAR`     | int      | Calendar year (2024–2025)               |
| `MO`       | int      | Calendar month (1–12)                   |
| `DY`       | int      | Calendar day (1–31)                     |
| `HR`       | int      | Hour of day (0–23)                      |

### Wind-field state

| Column  | Unit | Description                                           |
| ------- | ---- | ----------------------------------------------------- |
| `WS50M` | m/s  | Wind speed at 50 m height                             |
| `WD50M` | °    | Wind direction at 50 m height (0° = North, clockwise) |

### Electrical measurements — PCC side (B60, 60 kV)

| Column | Unit | Description              |
| ------ | ---- | ------------------------ |
| `V60`  | V    | Voltage amplitude at B60 |
| `I60`  | A    | Current amplitude at B60 |

### Electrical measurements — WT side (B575, 575 V)

| Column | Unit | Description               |
| ------ | ---- | ------------------------- |
| `V575` | V    | Voltage amplitude at B575 |
| `I575` | A    | Current amplitude at B575 |

### Harmonic targets (reconstruction labels)

| Column    | Unit | Description         |
| --------- | ---- | ------------------- |
| `THDV60`  | %    | Voltage THD at B60  |
| `THDI60`  | %    | Current THD at B60  |
| `THDV575` | %    | Voltage THD at B575 |
| `THDI575` | %    | Current THD at B575 |

---

## Notes

1. **Irregular temporal spacing**
   - Most consecutive records are spaced by exactly 1 hour.
   - A small fraction of gaps are 2–10 hours. If you need a strictly regular hourly grid for sequence modeling, re-indexing and interpolation are required (as done in the accompanying paper).
   - After re-indexing to a full hourly grid over the reported time range, the expected number of slots is 13,224; the raw file therefore contains approximately 1,500 missing hours.

2. **Physical consistency**
   - All THD, voltage, current, and wind-speed values are non-negative.
   - In every record, the turbine-side THD (`THDV575`, `THDI575`) is strictly larger than the corresponding PCC-side THD (`THDV60`, `THDI60`), reflecting the expected attenuation from the converter-dominated WT terminal toward the grid-side PCC.

3. **Train/validation/test split**
   - The paper uses a **chronological split** (70% / 15% / 15%) to avoid future information leakage.
   - Do **not** use a random shuffle split for time-series experiments.

4. **Preprocessing used in the paper**
   - Trigonometric encoding of `WD50M`, hour, month, and day-of-year.
   - Z-score normalization of continuous inputs (statistics computed on the training split only).
   - Construction of auxiliary equivalent-harmonic-intensity variables: `E = V × THD` (amplitude-scaled).
   - Sliding-window sample generation with window lengths `L ∈ {24, 72}`.

---

## License

This dataset is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.

You are free to:

- **Share** — copy and redistribute the material in any medium or format.
- **Adapt** — remix, transform, and build upon the material for any purpose, even commercially.

Under the following terms:

- **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made.

See `LICENSE` for the full legal text.

---

## Citation

If you use this dataset in your research, please cite the associated paper:

```bibtex
@article{electric2025pinn,
  title={PINN-Inspired Topology-Aware Learning for Harmonic State Reconstruction in Offshore Wind Farms},
  journal={To be determined},
  year={2025},
  note={Dataset available with this paper}
}
```

A ready-to-use BibTeX entry is also provided in `CITATION.bib`.


---

## Contact

For questions regarding this dataset or the associated code, please open an issue in the project repository.
