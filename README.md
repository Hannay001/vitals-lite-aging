# Vitals‑Lite‑Aging — synthetic vitals mini‑dataset + 10‑min Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Hannay001/vitals-lite-aging/blob/main/notebooks/demo.ipynb)

A small, fully **synthetic** vitals table (2,500 rows, 10 features + 2 labels) plus a **10‑minute Colab** notebook for teaching bio/health ML with **no PHI**.  
**Goal:** a tiny, reproducible public good — one CSV, one Colab, one figure.

---

## Dataset at a glance
- **Rows:** 2,500 | **Columns:** 13 (`row_id` + 10 features + 2 labels)
- **Features:** `age_years`, `sex`, `height_cm`, `weight_kg`, `bmi`, `resting_hr_bpm`, `systolic_bp_mmHg`, `diastolic_bp_mmHg`, `temp_c`, `spo2_pct`
- **Labels:**
  - `age_65plus` = 1 if `age_years ≥ 65`, else 0
  - `hypertension_flag` = 1 if `systolic_bp_mmHg ≥ 130` **or** `diastolic_bp_mmHg ≥ 80`, else 0
- **Constraint enforced:** `diastolic_bp_mmHg < systolic_bp_mmHg` (row‑wise)

> **Note:** This dataset is synthetic and for **education/demo** only. It is **not** for clinical use.

---

## Quickstart
1. Click the **Colab badge** above (after you adjust USER/REPO) or open `notebooks/demo.ipynb` directly.
2. Run all cells. The notebook loads `data/vitals_lite.csv`, trains a small **LogisticRegression** on `age_65plus`, prints accuracy + a confusion matrix, and draws one example figure.

### Local (optional)
```bash
pip install -r requirements.txt
# or minimal:
python -m pip install sdv sdmetrics scikit-learn pandas numpy matplotlib
```

---

## Evaluation & QA
- **SDMetrics QualityReport (single‑table): overall ≈ `0.926`** (0–1 scale; higher means closer match of column distributions & pairwise trends).  
  Load the artifact:
  ```python
  from sdmetrics.reports.single_table import QualityReport
  qr = QualityReport.load('quality_report.pkl')
  print(qr.get_score())
  ```
- **DiagnosticReport:** “Data Validity” and “Data Structure” near 1.0 when metadata and columns align.
- **Constraint proof:** All rows satisfy `DBP < SBP` via an inequality constraint during synthesis.

**Artifacts included:**
- `quality_report.pkl` (saved report)
- `data/vitals_lite.csv` (dataset)

---

## Data dictionary (short)
- `row_id` (id): unique row identifier
- `age_years` (numeric): 18–90
- `sex` (categorical): male/female
- `height_cm` (numeric): ~150–195
- `weight_kg` (numeric): ~45–120
- `bmi` (numeric): computed: `weight_kg / (height_cm**2) * 10000`
- `resting_hr_bpm` (numeric): ~60–100
- `systolic_bp_mmHg` (numeric): ~90–150+
- `diastolic_bp_mmHg` (numeric): ~50–110
- `temp_c` (numeric): ~36.1–37.6 (fever ≥ 38.0)
- `spo2_pct` (numeric): ~95–100%
- `age_65plus` (categorical): binary label
- `hypertension_flag` (categorical): binary label

---

## Licensing
- **Code:** MIT License
- **Data & docs:** CC BY 4.0

---

## How to cite
“Vitals-Lite-Aging (v0.1.0)” — https://github.com/Hannay001/vitals-lite-aging  
Code: MIT • Data/Docs: CC BY 4.0

---

## Release notes
**v0.1.0**
- First public release: dataset CSV, demo notebook, and saved `quality_report.pkl`.
