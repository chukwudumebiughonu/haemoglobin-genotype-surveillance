# Dataset Validity and Provenance

This document describes the source, integrity, and validity of the analytical dataset
used in the manuscript *Ten-Year Haemoglobin Genotype Surveillance in a Nigerian
University Cohort (Bowen University, 2015–2024)*. It is provided to support the
editorial requirement that the validity of the datasets used in the study be
demonstrated and the access links provided.

**Repository:** https://github.com/chukwudumebiughonu/haemoglobin-genotype-surveillance
**Archived release (DOI):** [https://doi.org/10.5281/zenodo.20445528](https://doi.org/10.5281/zenodo.20451392)

---

## Source of the data

The dataset comprises de-identified haemoglobin genotype screening records of newly
admitted students at Bowen University, Iwo, Osun State, Nigeria, collected as part of
the institution's routine pre-matriculation medical screening between 2015 and 2024.
Genotype was determined by haemoglobin electrophoresis at the Bowen University Medical
Centre. Sex and year of screening were recorded as part of the same screening exercise.

The dataset was obtained from institutional screening archives. The records contain no
direct personal identifiers (no names, no admission numbers, no addresses, no dates of
birth). Sex is recorded as M or F; year is recorded as the four-digit screening year;
genotype is recorded as one of six categories (HbAA, HbAS, HbAC, HbSS, HbSC, HbCC).

---

## Files released

The release contains both raw and processed forms of the data so that every step of the
cleaning pipeline is auditable:

| File | Description | Rows |
|---|---|---|
| `data/raw/Genotype_2015.csv` … `Genotype_2024.csv` | Annual screening exports as received | varies per year |
| `data/processed/processed_genotype_data.csv` | Cleaned and harmonised analytical dataset | 8,890 |
| `data/DATA_DICTIONARY.md` | Variable definitions, codings, and value ranges | — |

Combined raw rows: 8,890. No records were excluded between the raw exports and the
analytical dataset.

---

## Variables in the analytical dataset

| Variable | Type | Allowed values |
|---|---|---|
| `ID` | string | Internal record id (no link to personal identity) |
| `Sex` | string | `M` or `F` |
| `Genotype` | string | `AA`, `AS`, `AC`, `SS`, `SC`, `CC` |
| `Year` | integer | 2015 to 2024 |
| `Clinical_Category` | string | `Normal` (AA), `Carrier` (AS, AC), `Disease` (SS, SC, CC) |

The `Clinical_Category` field is derived directly from `Genotype` and is included for
convenience in the descriptive and inferential analyses.

---

## Integrity checks performed

The following quality checks were applied during the construction of the analytical
dataset and are re-run as assertions in Notebook 01 each time the pipeline is executed.
Failure of any assertion stops the pipeline:

1. **Total record count.** The analytical dataset must contain 8,890 rows (the sum of
   the annual exports).
2. **Year coverage.** Every year from 2015 to 2024 inclusive must be present, with no
   gaps and no spurious years.
3. **Sex completeness.** The `Sex` field is complete in 100% of analytical rows and
   takes only the values `M` or `F`.
4. **Genotype completeness.** The `Genotype` field is complete in 100% of analytical
   rows and takes only the six allowed values.
5. **Clinical-category consistency.** Every record's `Clinical_Category` matches the
   deterministic mapping from its `Genotype`.
6. **Identifier completeness.** The `ID` field is present in 8,877 of 8,890 records;
   the 13 records with missing `ID` are retained because all analytical fields are
   complete for them, and no analysis depends on `ID`.

These six checks are codified in Notebook 01 (`01_environment_and_loading.ipynb`) as
Python `assert` statements; the notebook prints a confirmation line for each.

---

## Ethical and access considerations

The screening dataset is de-identified at source. The raw annual exports and the
processed analytical dataset are released alongside the analysis code in the public
Zenodo archive (DOI above), under terms that permit re-use for academic and research
purposes subject to the licence stated in the repository and to applicable institutional
data-sharing agreements. No direct or indirect personal identifiers are released.

---

## Manuscript numbers reproducible from this dataset

The following numeric findings reported in the manuscript are exactly reproducible from
the released dataset via the published notebook sequence:

- Cohort size: **n = 8,890** records; female 57.57%, male 42.43%.
- Overall genotype distribution: HbAA 75.11%, HbAS 20.40%, HbAC 3.28%, HbSS 0.71%,
  HbSC 0.47%, HbCC 0.02%.
- Clinical categories: Normal 75.11%, Carrier 23.69%, Disease 1.20%.
- OLS trends (slope per year, R-squared, p-value):
  HbAA +0.3085, 0.16, 0.2512; Carrier −0.2782, 0.13, 0.3060;
  Disease −0.0303, 0.07, 0.4512.
- Chi-square tests:
  Sex × Genotype: χ² = 1.4864, df = 5, p = 0.9146.
  Sex × Clinical Category: χ² = 0.3891, df = 2, p = 0.8232.
- K-means clustering (k = 3, standardised carrier rate, disease rate, year):
  Stabilisation (2020, 2022, 2023); Transition (2017, 2019, 2021, 2024);
  High-burden (2015, 2016, 2018).

Any reader can reproduce these numbers in under one minute by running the notebook
sequence from a clean Python environment.

---

## Software environment

The full software environment is pinned in `requirements.txt`. Key versions: Python
3.11, pandas 2.0.3, numpy 1.24.3, scipy 1.11.1, scikit-learn 1.3.0, matplotlib 3.7.2,
seaborn 0.12.2, statsmodels 0.14.0, jupyterlab 4.0.3, openpyxl 3.1.2.

All random seeds are fixed (`random_state = 42`). The pipeline is deterministic.
