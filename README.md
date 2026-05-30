# Ten-Year Haemoglobin Genotype Surveillance in a Nigerian University Cohort

A reproducible analysis of haemoglobin genotype distribution among 8,890 university
students screened between 2015 and 2024 at Bowen University, southwestern Nigeria.
The study quantifies disease and carrier burden, tests for temporal trends, and uses
unsupervised learning to identify distinct epidemiological periods across the decade.

> **Status:** Manuscript accepted pending production requirements at *Scientific Reports*.
> Code and data archived with a DOI (see [Data and code availability](https://doi.org/10.5281/zenodo.20451392)).

## Key findings

- **HbAA (normal)** accounted for **75.11%** of the cohort (n = 6,677), with **HbAS** at
  **20.40%** and **HbAC** at **3.28%**.
- **Sickle cell disease genotypes (HbSS + HbSC)** were present in **1.18%** of students,
  below national estimates of 1.5–3%, consistent with survival and admission selection
  effects in a university population.
- **No statistically significant temporal trend** in any genotype category over the ten
  years (HbAA p = 0.25; carrier p = 0.31; disease p = 0.45), indicating a stable
  distribution.
- **K-means clustering** of the yearly aggregates identified **three epidemiological
  periods** — a stabilization period (2020, 2022, 2023), a transition period (2017, 2019,
  2021, 2024), and a high-burden period (2015, 2016, 2018) — revealing structure that a
  trend line alone does not capture.

## Repository structure

```
.
├── data/
│   ├── raw/                         Original yearly screening exports (2015-2024)
│   ├── processed/
│   │   └── processed_genotype_data.csv   Cleaned analytical dataset (8,890 rows)
│   └── DATA_DICTIONARY.md
├── notebooks/
│   ├── 01_environment_and_loading.ipynb
│   ├── 02_descriptive_exploration.ipynb
│   ├── 03_visualisation.ipynb
│   ├── 04_temporal_trend_analysis.ipynb
│   ├── 05_kmeans_clustering.ipynb
│   └── 06_correlation_analysis.ipynb
├── outputs/
│   ├── figures/                     Publication figures (300 DPI PNG + PDF)
│   └── tables/                      Every displayed table exported as CSV
├── docs/
│   ├── DECISIONS.md                 Methodology decisions and rationale
│   ├── FIGURES_PROVENANCE.md        Each manuscript figure mapped to its notebook cell
│   └── DATA_QUALITY_LOG.md          Cleaning steps and issues resolved
├── manuscript/
├── requirements.txt
├── CITATION.cff
└── LICENSE
```

## Methods at a glance

The analytical dataset holds one row per screened student with standardized identifier,
sex, genotype, year, and a derived clinical category (Normal = HbAA; Carrier = HbAS, HbAC;
Disease = HbSS, HbSC, HbCC). Annual trends were assessed with ordinary-least-squares
linear regression, associations between sex and genotype with chi-square tests of
independence, and pairwise feature relationships with Pearson correlation. Temporal
structure was explored with k-means clustering on the standardized yearly aggregates
(carrier rate, disease rate, year), with k = 3 selected by the elbow method and silhouette
analysis. All randomized steps use a fixed seed (`random_state = 42`).

## Reproducing the analysis

```bash
git clone <repository-url>
cd haemoglobin-genotype-surveillance
python -m venv .venv && source .venv/bin/activate    # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab
```

Run the notebooks in numerical order, 01 through 06, each from a clean kernel. Notebook 01
loads and validates the analytical dataset; 02 produces the descriptive tables; 03 the
figures; 04 the trend statistics; 05 the clustering; 06 the correlation analysis. Every
table is written to `outputs/tables/` and every figure to `outputs/figures/`. Outputs are
deterministic across runs given the pinned package versions in `requirements.txt`.

## Data and code availability

The de-identified analytical dataset and all analysis code are archived with a permanent
DOI: **[DOI to be inserted once minted]**. No personally identifiable information is
included. The raw screening exports are provided in `data/raw/` for transparency; the
cleaned analytical dataset in `data/processed/` is the authoritative input to all
notebooks.

## Publication

Adeleke TO, Ughonu CP, Agelebe E, et al. *Temporal Patterns of Haemoglobin Genotypes in
Nigeria: A Ten-Year Analysis of a University Haemoglobin Genotype Surveillance Data.*
Scientific Reports (in production). DOI: [to be inserted].

## Author contribution to this repository

Data curation, statistical analysis (descriptive statistics, chi-square testing, OLS trend
analysis, k-means clustering), figure production, and reproducible code authorship.

## License

Code is released under the MIT License (see `LICENSE`). The dataset is shared for the
purpose of reproducing the published analysis and remains subject to the ethics approval
under which it was collected.
