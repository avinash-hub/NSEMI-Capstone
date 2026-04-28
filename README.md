# NSEMI: National Semiconductor Ecosystem Maturity Index for India

**Walsh College | QM640 Data Analytics Capstone | Term 3 | April 2026**  
**Author:** Avinash Kashi Venugopal  
**Mentor:** Arun Kumar

## Project Overview

This capstone constructs the National Semiconductor Ecosystem Maturity Index (NSEMI) for India — a multi-dimensional composite index spanning four sub-indices: ecosystem depth, infrastructure reliability, workforce readiness, and cost competitiveness. The work extends Wang & Nhieu (2024)'s static CRITIC-TOPSIS framework into a continuously updatable time-series index, benchmarked against Taiwan, South Korea, Malaysia, Vietnam, and other comparator economies.

## Research Questions

- **RQ1: Ecosystem Depth** — Do Import Dependency Ratios for HS 3818, 8486, 8541, 8542 predict India's IIP Division 26 output? Which segment is the binding constraint?
- **RQ2: Infrastructure Reliability** — Does power infrastructure reliability differ between ISM-approved and non-approved Indian states?
- **RQ3: Workforce Readiness** — How does India's tertiary engineering enrollment compare to comparator semiconductor economies?
- **RQ4: Cost Competitiveness** — How does India's Composite Cost Index (CCI) compare to Asian semiconductor hubs, and what subsidy rate achieves cost parity?

## Methodology (Option A — 7 core methods)

| RQ | Approach | Statistical | ML |
|----|----------|-------------|-----|
| RQ1 | Stat + ML | Multiple Regression with Newey-West HAC; HHI | SHAP values |
| RQ2 | Statistical | Mann-Whitney U + descriptive comparison | — (future work) |
| RQ3 | Statistical | Welch's t-test + linear trend projection | — (future work) |
| RQ4 | Stat + ML | One-sample t-test + break-even analysis | PCA (CCI construction) |

## Data

The repository contains 32 datasets across 20 official sources plus 1 supplementary AT&C losses extraction:

- **28 API/scraped extractions** with complete provenance logs in `data/provenance/`
- **4 compiled reference tables** drawn from cited literature, labeled `compiled_reference_*` in their `data_source` column
- **1 supplementary dataset** (`rq2_pfc_atc_losses.csv`) extracted from the PFC *Report on Performance of Power Utilities 2024-25*, providing state-level AT&C (Aggregate Technical & Commercial) loss percentages for RQ2 hypothesis testing

API domains: api.worldbank.org, comtradeapi.un.org, api.data.gov.in, archive-api.open-meteo.com, data.uis.unesco.org, databrowser.uis.unesco.org, powermin.gov.in, tradestat.commerce.gov.in, www.rbi.org.in

See `data/cleaned/cleaning_report.csv` for the full cleaning operations log across all 32 files.

## Repository Structure