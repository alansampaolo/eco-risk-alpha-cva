# Eco-Risk Alpha: ESG, CDS Spreads and Counterparty CVA

This project investigates whether stronger ESG performance is associated with lower corporate credit risk and whether this relationship translates into lower counterparty Credit Valuation Adjustment (CVA).

The analysis combines econometric modeling, machine learning and counterparty credit risk calculations using a synthetic but realistic corporate dataset.

## Project Overview

The project addresses three main questions:

1. Do ESG variables provide meaningful information about corporate CDS spreads beyond traditional financial fundamentals?
2. Can this relationship affect the computation of counterparty CVA?
3. Is there evidence of a systematic green premium across firms?

The analysis is based on a benchmark portfolio consisting of a 5-year payer Interest Rate Swap (IRS).

The workflow proceeds through:

- data cleaning and preprocessing;
- exploratory data analysis;
- OLS regression with heteroskedasticity robust standard errors;
- Random Forest and Gradient Boosting models;
- conversion of CDS spreads into hazard rates and survival probabilities;
- CVA computation;
- comparison between market-based and model-implied CVA.

## Data

The project uses three datasets:

- `companies_data.csv` — company level data including ESG variables, financial fundamentals and observed 5-year CDS spreads;
- `irs_expected_exposure.csv` — Expected Exposure profile for the 5-year payer IRS;
- `eur_zero_rates.csv` — EUR zero rate term structure used to compute discount factors.

The raw corporate dataset contains approximately 60 anonymized firms.

The available company-level variables include:

- ESG Score;
- Environmental, Social and Governance pillar scores;
- Carbon Intensity;
- Market Capitalization;
- Total Assets;
- Debt-to-Equity Ratio;
- EBITDA Margin;
- Leverage Ratio;
- Sector;
- Region;
- observed 5-year CDS Spread.

## Data Preparation

The preprocessing stage includes:

- standardization of categorical labels;
- removal of duplicates;
- treatment of missing values;
- reconstruction of missing ESG scores using:

`ESG = 0.5 * E + 0.3 * S + 0.2 * G`

- sector wise median imputation for selected missing financial variables;
- log transformation of Market Capitalization and Carbon Intensity to reduce skewness and the influence of extreme values.

After preprocessing, approximately 50 observations are used in the main modeling exercises.

## Exploratory Analysis

The exploratory analysis examines the relationship between CDS spreads and both ESG and financial variables.

Scatter plots and correlation matrices show no clear linear relationship between ESG Score and CDS spreads.

Traditional financial fundamentals, particularly firm size, leverage and profitability measures, display stronger associations with CDS spreads than ESG variables.

## Econometric Baseline

A linear regression is estimated as a benchmark:

`CDS = alpha + beta1 * ESG + beta2 * Total Assets + beta3 * Leverage + beta4 * EBITDA Margin + epsilon`

White heteroskedasticity-robust standard errors are used for inference.

The ESG coefficient is close to zero and statistically insignificant.

The linear model also exhibits limited explanatory and predictive power, suggesting that a simple linear specification is not sufficient to capture the structure of CDS spreads in the sample.

## Machine Learning Models

Two non-linear ensemble models are estimated:

- Random Forest;
- Gradient Boosting.

The models use:

- ESG Score;
- log Market Capitalization;
- log Carbon Intensity;
- Leverage Ratio;
- EBITDA Margin;
- Sector dummies;
- Region dummies.

Model performance is evaluated using 5-fold cross-validation.

Both models show limited predictive performance, partly reflecting the small size of the available sample.

Feature importance and partial dependence analysis indicate that traditional financial fundamentals, especially firm size and leverage, are more influential than ESG variables.

The marginal effect of ESG Score on predicted CDS spreads is weak and largely flat in both models.

## From CDS Spreads to CVA

Observed and predicted CDS spreads are translated into counterparty credit risk measures.

The workflow is:

`CDS Spread -> Hazard Rate -> Survival Probability -> CVA`

The hazard rate is approximated as:

`lambda = CDS Spread / (1 - R)`

where `R` is the recovery rate.

The survival probability at time `t` is:

`S(t) = exp(-lambda * t)`

The project assumes a Loss-Given-Default of:

`LGD = 60%`

Discount factors are obtained from the EUR zero-rate curve using continuous compounding.

The unilateral CVA is approximated as:

`CVA ≈ LGD * sum(EE_i * DF_i * (S_(i-1) - S_i))`

where:

- `EE_i` is the expected exposure;
- `DF_i` is the discount factor;
- `S_i` is the survival probability.

## CVA Measures

Three CVA series are computed:

- `CVA_actual` — based on observed CDS spreads;
- `CVA_RF` — based on Random Forest predicted CDS spreads;
- `CVA_GB` — based on Gradient Boosting predicted CDS spreads.

## Green Premium Analysis

In the original project framework, the green premium is defined as:

`Green Premium = CVA_actual - CVA_predicted`

The distribution of this difference is analyzed for both Random Forest and Gradient Boosting.

The resulting distributions are heterogeneous across firms and approximately centered around zero.

The analysis therefore does not provide evidence of a systematic green premium across the sample.

### Interpretation Note

The difference between market-based and model-implied CVA should be interpreted with caution.

Because the predictive models include both ESG variables and traditional financial fundamentals, the quantity

`CVA_actual - CVA_predicted`

does not isolate a pure ESG effect.

It can be interpreted more generally as a market-model CVA gap.

The marginal role of ESG is assessed more directly through:

- the ESG coefficient in the OLS regression;
- feature importance measures;
- Partial Dependence Plots;
- Individual Conditional Expectation plots.

These diagnostics consistently indicate a weak and non-systematic ESG contribution.

## Main Findings

- ESG Score shows little direct association with observed CDS spreads.
- The ESG coefficient in the OLS regression is statistically insignificant.
- Random Forest and Gradient Boosting also assign limited importance to ESG variables.
- Firm size, leverage and profitability indicators are more relevant drivers of CDS spreads.
- The predictive models exhibit limited out-of-sample performance due to the small dataset.
- Model-implied and market-based CVA differences are heterogeneous across firms and centered around zero.
- No systematic green premium is identified in the sample.
- The project highlights the importance of data quality and sample size when studying the relationship between sustainability and credit risk.

## Repository Structure

```text
eco-risk-alpha-cva/
│
├── README.md
├── eco_risk_alpha_analysis.ipynb
├── eco_risk_alpha_report.pdf
├── companies_data.csv
├── irs_expected_exposure.csv
└── eur_zero_rates.csv

## Main Files

- `eco_risk_alpha_analysis.ipynb` — complete Python workflow including data cleaning, econometric analysis, machine learning models and CVA computation
- `eco_risk_alpha_report.pdf` — full academic report with methodology, results and discussion
- `companies_data.csv` — corporate ESG, financial and CDS data
- `irs_expected_exposure.csv` — expected exposure profile of the 5-year payer IRS
- `eur_zero_rates.csv` — EUR zero-rate curve used for discounting

## Technologies

- Python
- pandas
- NumPy
- scikit-learn
- statsmodels
- matplotlib
- seaborn
- Econometrics
- Machine Learning
- Credit Risk
- Counterparty Risk
- CVA
- Sustainable Finance

## Limitations

The study is based on a small synthetic cross-sectional dataset.

This limits statistical power and predictive performance and makes it difficult to detect potentially subtle ESG effects.

The results should therefore be interpreted as a methodological exercise rather than as conclusive evidence on the existence or absence of a green premium in real financial markets.
