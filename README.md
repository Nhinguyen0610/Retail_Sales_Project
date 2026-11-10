# Determinants of Retail Sales: Statistical Analysis of Car Seat Sales across 400 store
This repository contains the full analysis pipeline for a retail analytics project that identifies the key drivers of **car seat unit sales** using the **Carseats datasets**. The project progresses from data quality checks and exploratory visualisation, through hypothesis testing on advertising and price sensitivity to a multiple linear regression model that quantifies the effect of each store and market characteristic on sales, followed by a full check of the regression assumptions.
## Dataset
- **Source:** Carseats dataset from *An Introduction to Statistical Learning* (ISLR), a simulated dataset of car seat sales at 400 stores.
- **Content:** 400 observations and 11 variables covering sales, pricing (own and competitor), local advertising budget, community demographics (income, population, age, education), shelf location quality (Bad / Medium / Good), and store location (Urban, US).
- **Data quality:** 4 missing values (one each in `Sales`, `CompPrice`, `Income`, `Urban`), imputed with the median for numerical variables and the mode for the categorical variable. No duplicated records.
## Files:
| File | Purpose |
| ---- | ------- |
| `Retail_Sales_Analysis.ipynb` | Full analysis notebook: data quality checks, exploratory data analysis, categorical frequency analysis, Chi-square test, segmented t-tests, grouped analysis, OLS regression and assumption diagnostics. |
| `Carseats.xlsx` | Raw dataset used by the notebook. |
| `Retail_Sales_Report.pdf` | Exported version of the notebook with all outputs, charts and written interpretation. |
## Pipeline Overview

```
Carseats.xlsx (400 stores, 11 variables)
        │
        ▼
Data quality checks        → missing value imputation, duplicate check
        │
        ▼
Exploratory analysis       → distributions, scatter plots, boxplots, correlation heatmap
        │
        ▼
Hypothesis testing         → Chi-square (Advertising level × US)
        │                    one-tailed t-tests (Price effect within income groups)
        ▼
Grouped analysis           → Sales and Price by shelf location
        │
        ▼
OLS regression             → effect sizes for each driver of Sales
        │
        ▼
Model diagnostics          → VIF, residuals vs fitted, Q-Q plot, residual histogram
```
## Setup

1. Clone the repository:

   ```
   git clone https://github.com/Nhinguyen0610/Retail_Sales_Project.git
   cd Retail_Sales_Project
   ```

2. Install the dependencies:

   ```
   pip install pandas numpy matplotlib seaborn statsmodels scipy openpyxl
   ```

3. Make sure `Carseats.xlsx` is in the same folder as the notebook, then launch Jupyter and run all cells in order:

   ```
   jupyter lab
   ```

## Methods Implemented

**Exploratory data analysis:**

- Histogram of Sales distribution
- Scatter plots of Sales against Price, Income and Advertising
- Boxplots of Sales by US, Urban and ShelveLoc
- Price–Sales scatter plot by shelf location
- Correlation heatmap (categorical variables dummy-encoded)

**Statistical testing:**

- Chi-square test of independence between advertising level (median split) and US location
- One-tailed independent-samples t-tests comparing Sales in high-price vs low-price stores, run separately for high-income and low-income areas

**Regression modelling:**

- Multiple linear regression (OLS): `Sales ~ CompPrice + Income + Advertising + Price + Age + ShelveLoc`
- Multicollinearity check with Variance Inflation Factor (VIF)
- Assumption checks for linearity, constant variance and normality of residuals

## Key Findings

- The regression model explains **86.9%** of the variation in sales (Adjusted R² = 0.866), with all predictors significant at the 5% level.
- **Shelf location** is the strongest driver: Good placement is associated with about 4,805 more units sold than Bad placement, and Medium with about 1,922 more.
- **Price** and **competitor price** have almost equal and opposite effects (about −95 and +93 units per $1), suggesting customers respond mainly to the price gap with competitors.
- **Advertising** adds about 115 units per $1,000 spent, and is strongly associated with US location (Chi-square, p < 0.001).
- Lower-priced stores sell significantly more in both high- and low-income areas (one-tailed t-tests, p < 0.001).

## Notes

- `Sales` is measured in thousands of units, so a coefficient of 4.805 corresponds to about 4,805 units.
- The data are cross-sectional, so the regression identifies associations rather than causal effects.
- Income, Price and Advertising are split at the median for the segment-level tests; this simplifies continuous variables and may hide finer patterns. Interaction effects (e.g. price sensitivity by shelf location) are not modelled.
