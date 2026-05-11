# NSEMI: National Semiconductor Ecosystem Maturity Index for India

**Walsh College — QM640 Data Analytics Capstone (Spring 2026)**
**Author:** Avinash Kashi Venugopal
**Mentor:** Arun Kumar

A continuously updatable, segment-disaggregated, time-series composite indicator that extends the Wang and Nhieu (2024) static cross-sectional CRITIC-TOPSIS framework into a measurement instrument suitable for India's rapidly evolving semiconductor industrial policy.

---

## Project Overview

India has committed **₹1.59 lakh crore** in semiconductor investments under the India Semiconductor Mission (ISM), with 95% concentrated in just two states (Gujarat and Assam). This capstone asks: **where are India's binding ecosystem constraints — and which can be solved with capital versus which require institution-building?**

The NSEMI composite has four structural sub-indices, each addressed by a dedicated research question:

| Sub-Index | Research Question | Primary Test | Headline Finding |
|-----------|-------------------|--------------|------------------|
| **SI₁** Ecosystem Depth | RQ1 — Which import-dependency segment binds monthly production? | OLS + Newey-West HAC + SHAP | H₀ rejected (F=3.98, p=0.005); ICs lead, not Equipment |
| **SI₂** Infrastructure Reliability | RQ2 — Do ISM-approved states differ on power infrastructure? | Mann-Whitney U + rank-biserial r | Directional H₁; medium effect on T&D (r=+0.38) |
| **SI₃** Workforce Readiness | RQ3 — Is there a structural enrollment gap vs Asian hubs? | Welch's t + Wilcoxon + Linear Trend + WGI | H₀ rejected (p<10⁻⁶, d=−1.24); 2030 WGI=1.58 |
| **SI₄** Cost Competitiveness | RQ4 — What subsidy rate achieves cost parity? | Equal-weighted CCI + Wilcoxon | H₀ rejected (p=0.001, d=+3.31); break-even s*=18.5% |

---

## Key Findings at a Glance

- **RQ1 (Import Dependency).** Synopsis-predicted leadership of HS 8486 (Equipment) **not borne out** in monthly regression. The largest coefficient belongs to HS 8542 (Integrated Circuits) at β = +397.52 (p = 0.015), with HS 3818 (Raw Materials) as the second significant predictor (β = +33.76, p = 0.003) and HHI > 7,000 indicating single-supplier vulnerability. Reframed conclusion: the binding constraint is **horizon-dependent** — ICs at the monthly margin, Equipment and Raw Materials at the investment-cycle margin.

- **RQ2 (Infrastructure).** Both H₀ variables show the predicted direction; neither rejects at α = 0.05 due to small-n₁ (n₁ = 6). T&D loss shows a **medium effect** (r = +0.379, one-tailed p = 0.085); power deficit shows a negligible effect (saturated near zero across India). SI₂ composite places **3 of 6 ISM-approved states in the top-10** of all 28 states (Gujarat #2, Punjab #4, Andhra Pradesh #8).

- **RQ3 (Workforce).** India's tertiary enrollment ratio (mean 30.28%) is statistically below the comparator hubs (mean 59.87%) with **Cohen's d = −1.243 (large effect)**, p < 0.000001. India's AICTE engineering intake is **contracting at approximately 47,900 seats per year** (R² = 0.89). Projected **2030 WGI = 1.58**, meaning IESA-projected demand exceeds total engineering-seat supply by 58%.

- **RQ4 (Cost).** India ranks **#1 of 8 most expensive** countries in the panel, approximately 0.93 z-score units above the hub median. **Cohen's d = +3.31** — one of the largest standardised effect sizes observable. **Break-even subsidy rate s* = 18.5%**, well below the ISM Modified Programme statutory ceiling of 50%. At 30%, 50%, or 70% subsidy, India ranks cheapest of all 8 countries.

- **Cross-cutting policy implication.** Cost is solvable with money (s* well below ceiling). Workforce and infrastructure require institution-building on slower timescales that capital alone cannot accelerate.

---

## Repository Structure

```
NSEMI-Capstone/
├── notebooks/                              # Four research-question notebooks
│   ├── RQ1_Analysis_Modelling.ipynb       # OLS + HAC + SHAP
│   ├── RQ2_Analysis_Modelling.ipynb       # Mann-Whitney U + SI₂
│   ├── RQ3_Analysis_Modelling.ipynb       # Welch's t + Linear Trend + WGI
│   └── RQ4_Analysis_Modelling.ipynb       # PCA adequacy + CCI + Break-even
├── data/
│   ├── RQ1/{Raw, cleaned}/                # DGFT MEIDB, MOSPI IIP, UN Comtrade
│   ├── RQ2/{Raw, cleaned, PDFs}/          # CEA, PFC, SERC, ISM PIB
│   ├── RQ3/{Raw, cleaned}/                # WB SE.TER.ENRR, AICTE, IESA
│   └── RQ4/{Raw, cleaned}/                # WB lending, Tax Foundation, WB DB archive
├── results/
│   ├── RQ1/Modelling/                     # OLS coefficients, SHAP ranking, diagnostics
│   ├── RQ2/Modelling/                     # MW-U results, SI₂ composite
│   ├── RQ3/Modelling/                     # Welch's t, linear trend, WGI
│   └── RQ4/Modelling/                     # CCI panel, ranking, break-even, sensitivity
├── reports/                                # Interim report DOCX/PDF
├── .gitignore
└── README.md
```

**Note:** Large SERC Tariff Order scanned PDFs (>100 MB each) are excluded from the repository per `.gitignore`. They are deferred to the final report pending Tesseract OCR; source URLs are documented in the notebooks and final report.

---

## Data Sources

All seventeen primary datasets are public Government-of-India and international-organisation documents. **No synthetic data is used at any stage.** Provenance is preserved in JSON logs alongside each cleaned dataset.

| RQ | Sources |
|----|---------|
| **RQ1** | MOSPI eSankhyiki API (IIP Division 26), DGFT TRADESTAT MEIDB (monthly imports/exports per HS code), UN Comtrade (cross-country benchmark) |
| **RQ2** | CEA Executive Summary (deficit), CEA General Review Ch. 6 Table 6.4 (T&D loss), SERC Tariff Orders (10 states), PFC Performance 2024-25, ISM PIB releases (PRIDs 1983128, 2010132, 2050859, 2128605, 2155456), IMD Open-Meteo |
| **RQ3** | UNESCO UIS / World Bank SE.TER.ENRR, AICTE data.gov.in resource 57cf415f, IESA Semicon India Report milestones |
| **RQ4** | World Bank FR.INR.LEND + FR.INR.RINR, Tax Foundation corporate tax CSV, World Bank Doing Business archive (CC-BY-4.0), World Bank EG.USE.ELEC.KH.PC, PIB ISM subsidy PDFs |

---

## Methodology

Each RQ implements its **synopsis-locked Option A methodology**, validated against published thresholds:

1. **RQ1 — OLS with Newey-West HAC** at lag = 4 per Greene's monthly-data heuristic. ADF stationarity testing → first-differencing of all variables. SHAP via LinearExplainer for model-agnostic interpretability (synopsis novelty).

2. **RQ2 — Mann-Whitney U** (one-tailed, alternative='less') with rank-biserial r effect size per Cohen 1988 benchmarks. 28-state census; n₁ = 6 (mentor-approved deviation A, PIB Aug 2025), n₂ = 22.

3. **RQ3 — Welch's t-test** (unequal-variance, variance ratio = 92.4×) with Wilcoxon rank-sum fallback. Linear OLS for India trend regression. Deterministic Workforce Gap Index = ISM demand / forecasted supply.

4. **RQ4 — PCA-or-Equal-Weighted CCI.** KMO = 0.37 and PC1 = 44.6% both below thresholds → equal-weighted fallback per OECD Handbook §6. One-sample Wilcoxon signed-rank (Shapiro-Wilk on India CCI: p = 0.0066). Break-even s* with three-scenario sensitivity (30%, 50%, 70%).

---

## Mentor-Approved Deviations from Synopsis

Three unavoidable deviations were approved on May 3, 2026, each documented as primary-source-verification or data-availability constraints, not methodological compromises:

- **Deviation A (RQ2).** ISM-approved state count expanded from n₁ = 3 to n₁ = 6 per PIB Press Release ID 2155456 (August 2025).
- **Deviation B (RQ3).** H6 hierarchical regression DV substituted from semiconductor FDI by state to ISM-approved investment in INR crore by state, because DPIIT does not publish FDI at the state × sector intersection.
- **Deviation C (RQ4).** Taiwan excluded from the hub-median benchmark due to 1980 expulsion from IMF (April 17) and World Bank (May 1980) following UN Resolution 2758. Hub set reduced to South Korea, Malaysia, and Vietnam.

---

## Reproducing the Analysis

### Requirements

- Python 3.10+
- Jupyter Notebook or JupyterLab
- Core libraries: `pandas`, `numpy`, `scipy`, `statsmodels`, `scikit-learn`, `shap`, `matplotlib`, `seaborn`
- API access libraries: `requests`, `wbgapi`, `pdfplumber`

Install via:

```bash
pip install pandas numpy scipy statsmodels scikit-learn shap matplotlib seaborn requests wbgapi pdfplumber
```

### Running the Notebooks

```bash
# Clone the repo
git clone https://github.com/avinash-hub/NSEMI-Capstone.git
cd NSEMI-Capstone

# Launch Jupyter
jupyter notebook notebooks/

# Execute in order:
# 1. RQ1_Analysis_Modelling.ipynb
# 2. RQ2_Analysis_Modelling.ipynb
# 3. RQ3_Analysis_Modelling.ipynb
# 4. RQ4_Analysis_Modelling.ipynb
```

Each notebook is self-contained: it extracts data from the primary sources (live API calls + cached raw files), runs the cleaning pipeline, executes the modelling, and saves results to `results/RQn/Modelling/`. Notebooks fall back to cached raw data in `data/RQn/Raw/` when API endpoints return errors (the AICTE data.gov.in API returned HTTP 502 during extraction, so RQ3 uses the offline CSV mirror of the same resource ID 57cf415f).

---

## Interim Report

The full interim report is in `reports/QM640_Interim_Report_Final_Avinash_Venugopal.docx`. It contains:

- 9 sections following the QM640 capstone template (Introduction, Scope and Objectives, Literature Survey, Data Description, Analysis, Modelling, Preliminary Results, Bibliography, Appendix)
- 19 figures and 20 tables across the four RQs
- 37 APA-7 references
- Code appendices A–D corresponding to the four RQs

---

## Performance Evaluation Summary

| Diagnostic | RQ | Threshold | Observed |
|-----------|----|-----------|----------|
| Sample size | RQ1 | N ≥ 82 (Green's rule) | **97** ✓ |
| Sample size | RQ2 | Census of 28 | **28** ✓ |
| Sample size | RQ3 | N ≥ 35 (CI-based) | **48** ✓ |
| Sample size | RQ4 | N ≥ 35 (CI-based) | **80** ✓ |
| Primary H₀ test | RQ1 | p < 0.05 | **0.005** ✓ |
| Primary H₀ test | RQ2 (T&D) | p < 0.05 | 0.085 ✗ at α=0.05 |
| Primary H₀ test | RQ3 | p < 0.05 | **<10⁻⁶** ✓ |
| Primary H₀ test | RQ4 | p < 0.05 | **0.000977** ✓ |
| Effect size | RQ3 | Cohen's d ≥ 0.5 | **−1.243 (large)** ✓ |
| Effect size | RQ4 | Cohen's d ≥ 0.5 | **+3.314 (very large)** ✓ |

---

## Status and Next Steps

**Completed (Weeks 1–9):** Data extraction from 17 primary sources; cleaning across all four RQs; EDA with 19 figures; primary modelling for all RQs; interim report deliverable.

**In progress:** RQ3 H6 hierarchical regression with mentor-approved substitute DV; final-report Section 5.5 reporting blocks; cross-RQ NSEMI composite construction.

**Pending for final report:** RQ2 industrial HT tariff dimension restoration via Tesseract OCR on the CEA Tariff Book 2024 scanned PDF; AICTE coverage extension from FY 2016-17 to FY 2024-25; formal SI₃ and SI₄ construction with sensitivity testing.

---

## License

Code is shared under the MIT License for academic and research use. Data follows the licensing of each primary source (World Bank CC-BY-4.0; UN Comtrade public; Government of India open-data terms; Tax Foundation CC-BY-NC-4.0).

---

## Contact

**Avinash Kashi Venugopal**
Walsh College — Master of Science in Data Analytics
QM640 Data Analytics Capstone, Spring 2026 Term

For questions on methodology, data, or reproducibility, please open a GitHub issue or reference the corresponding section of the interim report.

---

## Acknowledgments

This capstone benefited from the guidance of GL mentor Arun Kumar throughout the synopsis-to-interim phase, particularly in approving the three unavoidable deviations described above and in shaping the synopsis-locked Option A methodology. The wider NSEMI framework builds on the foundational CRITIC-TOPSIS work of Wang and Nhieu (2024).
