# Figures and Tables Provenance

This document maps every figure and table presented in the manuscript and the
supplementary materials to the exact notebook, the exact source data file, and the
rendering code that produced it. Every figure and table is reproducible end-to-end by
running the notebook sequence (01 → 06) from a clean kernel against the analytical
dataset `data/processed/processed_genotype_data.csv`.

**Repository:** https://github.com/chukwudumebiughonu/haemoglobin-genotype-surveillance
**Archived snapshot (DOI):** https://doi.org/10.5281/zenodo.20445528

---

## Reproducibility chain

```
data/raw/Genotype_YYYY.csv                       (10 annual screening exports, 2015-2024)
        │
        ▼
data/processed/processed_genotype_data.csv       (Notebook 01 — cleaning & harmonisation)
        │
        ▼
outputs/tables/02_*.csv                          (Notebook 02 — descriptive tables)
        │
        ▼
outputs/figures/*.{png,pdf}                      (Notebooks 03-06 — figures, 300 DPI + vector PDF)
```

---

## Figures cited in the manuscript

| Manuscript label | File in repository | Notebook | Source table / data | Description |
|---|---|---|---|---|
| **Figure 1** | `outputs/figures/genotype_distribution_by_year.png` | 03 | `outputs/tables/02_genotype_percentages_by_year.csv` | Stacked 100% bar of the six genotypes across 2015-2024 |
| **Figure S1** | `outputs/figures/clinical_category_trends.png` | 04 | `outputs/tables/02_clinical_category_percentages_by_year.csv` | Annual prevalence of Normal, Carrier, and Disease categories, including the disease range 0.5%-1.7% cited in Results |
| **Figure S2** | `outputs/figures/genotype_distribution_by_sex.png` | 03 | `outputs/tables/02_genotype_percentages_by_sex.csv` | Grouped bar of genotype prevalence by sex |
| **Figure S3** | `outputs/figures/figure_6_clustering_3d.png` | 05 | yearly aggregates from `processed_genotype_data.csv` | Three-dimensional clustering visualisation (Year × Carrier Rate × Disease Rate) confirming the three temporal periods |

---

## Tables cited in the manuscript

| Manuscript label | Source data / notebook | Description |
|---|---|---|
| **Table 1a / 1b** | Notebook 02; derived from `data/processed/processed_genotype_data.csv` | Sociodemographic and overall genotype-distribution summary of the n = 8,890 cohort |
| **Table 2** | `outputs/tables/06_pearson_correlation_matrix.csv` (Notebook 06) | Pearson correlation values across the six study variables (Year, Genotype Severity, Clinical Severity, Male Proportion, Carrier Rate, Disease Rate) |
| **Table 3** | `outputs/tables/05_cluster_summary.csv` (Notebook 05) | K-means cluster grouping: years, mean carrier rate, mean disease rate per cluster |

---

## Statistical outputs cited in-text

Every numeric value reported in the Results section is reproducible from a CSV file in
`outputs/tables/`:

| Manuscript figure | Reproducible source |
|---|---|
| Sex × Genotype chi-square (χ² = 1.4864, df = 5, p = 0.9146) | `outputs/tables/04_chi_square_results.csv` (Notebook 04) |
| Sex × Clinical Category chi-square (χ² = 0.3891, df = 2, p = 0.8232) | `outputs/tables/04_chi_square_results.csv` (Notebook 04) |
| OLS trend regressions (slopes, R², p-values for HbAA, Carrier, Disease) | `outputs/tables/04_trend_regression_results.csv` (Notebook 04) |
| k-means elbow (WCSS) and silhouette scores | `outputs/tables/05_elbow_silhouette.csv` (Notebook 05) |
| Per-year cluster assignment | `outputs/tables/05_cluster_assignments.csv` (Notebook 05) |
| Mean carrier and disease rate per cluster | `outputs/tables/05_cluster_summary.csv` (Notebook 05) |

---

## Reproducibility statement

All figures are rendered deterministically. K-means clustering uses
`random_state = 42`, `n_init = 10`, `max_iter = 300`, `tol = 1e-4`, with the standardised
feature triple (Carrier Rate, Disease Rate, Year). The standardiser is `scikit-learn`'s
`StandardScaler`. Library versions are pinned in `requirements.txt`. Running the
notebooks in order from a clean kernel reproduces every figure file at 300 DPI PNG and
vector PDF.
