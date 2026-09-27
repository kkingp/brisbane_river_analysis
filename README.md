# Evaluating Physicochemical Drivers of River Flow Using Weighted Regression

## Overview

Originating as a project for STAT 425 at the University of Illinois, Urbana-Champaign, the project has since been updated with various 
new additions such as updated visualizations, a train/test split validation section, and an overall more thorough analysis.

This project investigates whether physicochemical properties of the Brisbane River — pH, dissolved oxygen, salinity, chlorophyll, 
and turbidity — are associated with average water speed. Using over 20,000 observations collected at 30-minute intervals from the 
Queensland Government Open Data Portal, a multiple linear regression model was fit, diagnosed, and refined using Weighted Least 
Squares to address heteroscedasticity. The final model's predictive performance was validated on a held-out test set.


**Key result:** All five predictors were statistically significant (p < 0.01). Dissolved oxygen and salinity were associated with 
slower water speeds, while pH and turbidity were associated with faster speeds. The model explained a modest share of variability 
(Adjusted R² ≈ 0.174), and held-out test validation (RMSE ≈ 12.02 cm/s, MAE ≈ 8.98 cm/s) confirmed this reflects genuine variability 
in river dynamics rather than overfitting.

Full write-up: [`report/Brisbane_River_Report.pdf`](./report/Brisbane_River_Report.pdf)
Full code and output: [`code/brisbane_river_analysis.Rmd`](./code/brisbane_river_analysis.Rmd)

## Methods
- **Data cleaning:** Complete-case analysis on missing values (temperature, dissolved oxygen, salinity, turbidity)
- **Multicollinearity check:** Correlation matrix + Variance Inflation Factor (VIF); removed Specific Conductance 
                              (~0.999 correlated with Salinity)
- **Model selection:** Manual p-value elimination and stepwise selection under BIC, converging on the same final model
- **Diagnostics:** Breusch–Pagan test, Q–Q plots, component-plus-residual plots, Box–Cox transformation check
- **Heteroscedasticity correction:** Weighted Least Squares (WLS), with outlier removal via studentized residuals
- **Predictive validation:** 80/20 train/test split, RMSE and MAE on held-out data

## Libraries Used
| Library | Purpose |
|---|---|
| `tidyverse` | Data loading, cleaning, and wrangling |
| `lmtest` | Breusch-Pagan test for heteroscedasticity |
| `car` | VIF, Box-Cox transformation, CR plots, outlier testing |
| `caTools` | Train/test split for predictive validation |
| `ggcorrplot` | Correlation heatmap visualization |
| `broom` | Tidy extraction of model coefficients for plotting |

## Repository Structure
```
├── code/      → R Markdown file, run alongside brisbane_water_quality.csv
├── data/      → contains brisbane_water_quality.csv 
├── figures/   → Exported plots referenced in the report
└── report/    → Final written report (PDF)
```

## Data
This project uses the [Water Quality Monitoring Dataset](https://www.kaggle.com/datasets/downshift/water-quality-monitoring-dataset) 
(Fedorov, 2024) on Kaggle, sourced originally from the [Queensland Government Open Data Portal](https://www.data.qld.gov.au/). 
The dataset is licensed under Apache 2.0. To reproduce the analysis, place `brisbane_water_quality.csv` in the same folder as 
`code/brisbane_river_analysis.Rmd` before running it, since the code reads the file using a relative path.

## Limitations & Future Work
The model explains a modest share of variability, likely due to unmeasured hydrological drivers (rainfall, tidal cycles, discharge 
volume) not present in the dataset, and unmodeled temporal structure (the Timestamp variable was excluded for simplicity). See the 
full report for a detailed discussion and suggested extensions (time-series methods, k-fold cross-validation).

## Works Cited
See the full reference list in [`report/Brisbane_River_Report.pdf`](./report/Brisbane_River_Report.pdf).
