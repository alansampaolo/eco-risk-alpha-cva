# Eco-Risk Alpha: ESG, CDS Spreads and Counterparty CVA

Team case study by Deloitte, Summer School "Data Science for Sustainable Finance and Economics" — HTW Berlin (2025) · Python

## Objective

The objective was to **test whether ESG characteristics provide incremental predictive information for CDS spreads beyond traditional financial fundamentals**. Any predictive relationship could then **motivate future causal analysis** to investigate whether ESG characteristics actually affect credit risk.

Observed and model-implied CDS spreads are then converted into **unilateral CVA** on a 5-year payer interest rate swap, to measure a potential "green premium" (actual CVA − model-implied CVA).

## Setup

| | |
|---|---|
| **Data** | About 60 synthetic firms (50 after cleaning): ESG and E/S/G pillar scores, carbon intensity, size, leverage, profitability, sector, region, 5-year CDS spread |
| **Models** | OLS with heteroskedasticity-robust standard errors · Random Forest · Gradient Boosting (5-fold cross-validation, feature importance, PDP and ICE plots) |
| **CVA** | CDS spread → hazard rate `λ = s / (1 − R)` → survival probabilities → `CVA ≈ LGD · Σ EE_i · DF_i · (S_(i−1) − S_i)`, with LGD = 60% and EUR zero-rate discounting |

## Key results

- **ESG adds no detectable information:** its correlation with CDS spreads is about zero, and its OLS coefficient is not significant (p = 0.98).
- **Fundamentals dominate:** in both tree-based models, market capitalisation and leverage are the most important features, while ESG ranks low.
- **No systematic green premium:** the gap between actual and model-implied CVA is heterogeneous across firms and centred around zero, and grouping firms by ESG tertile shows no monotonic pattern.
- **Limited predictive power overall:** no model beats the sample mean out of sample (negative cross-validated R²), reflecting the small dataset. A weak ESG signal here does not rule out a causal effect.

## Repository

- `eco_risk_alpha_analysis.ipynb` — full Python workflow (own implementation)
- `eco_risk_alpha_report.pdf` — individual report
- `companies_data.csv`, `irs_expected_exposure.csv`, `eur_zero_rates.csv` — input data provided for the case

**Tools:** Python · pandas · statsmodels · scikit-learn · Matplotlib · seaborn
