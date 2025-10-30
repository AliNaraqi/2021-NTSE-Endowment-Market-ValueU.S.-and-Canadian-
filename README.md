# NTSE 2021 Endowment Analysis

This project analyzes the 2021 NTSE endowment market values for U.S. and Canadian institutions. 
It cleans and enriches the dataset, computes descriptive statistics, builds visualizations, performs comparative/correlation analysis, and includes simple predictive and clustering models. An interactive Streamlit app is provided to explore institutions.

## Repository contents
Source data: https://www.nacubo.org/Research/2021/Public-NTSE-Tables

 
## Data preparation
- Normalized columns and coerced numerics from string-formatted numbers
- Derived fields:
  - `FY21_Endowment_$`, `FY20_Endowment_$` (in dollars)
  - `Pct_Change` (FY20→FY21) with backfill if missing
  - `FY21_Per_FTE_$` (computed if needed)
  - `Type` (Public/Private from IPEDS sector)
  - `Country` (U.S. vs Canada inferred from state/province codes)

## Key questions answered
- Aggregations (mean/median/sum) by `Country`, `State/Province`, `Type`, `Carnegie`
- Top/bottom institutions by FY21 endowment, % change, and wealth per FTE
- Geographic views (U.S. choropleth, Canada province bars)
- Distributions (histograms/boxplots) and scatterplots (FTE vs endowment)
- Correlations: endowment vs enrollment, % change vs FY20 base
- Growth analysis: absolute dollar change, fastest growers, negatives/stagnant
- Concentration: state/province totals, Carnegie shares, Top-10 vs rest
- Modeling: Ridge regression to predict % change; K-Means clustering on size/growth/per-FTE

## Summary conclusions (high level)
- Wealth concentration is substantial:
  - Top 10 institutions hold ~37% of total FY21 higher-ed endowment wealth in the dataset.
  - A handful of U.S. states (e.g., MA, CA, TX, NY) and Canadian provinces (e.g., ON, BC, QC) dominate totals.
- Institutional characteristics:
  - Private institutions tend to dominate the upper tail of absolute endowment values and per-FTE wealth.
  - Doctoral/Research categories (Carnegie) account for the majority of aggregate endowment.
- Size relationships:
  - Enrollment (FTE) and endowment show a positive association (Spearman ρ ~0.47), but with large dispersion—bigger schools do not always have bigger endowments.
  - % change vs FY20 base shows weak-to-moderate positive association in this period (larger endowments did not systematically underperform smaller ones during FY21).
- Growth patterns:
  - Fastest-growing institutions by dollars are large endowments; by percent, smaller or mid-sized can appear.
  - A small subset shows negative or near-zero growth; most institutions experienced positive gains in FY21.
- Clusters (k=4 example):
  - Distinct groups emerge along size (FTE/endowment) and per-FTE wealth dimensions; Canadian vs U.S. mix differs across clusters but clusters are primarily scale/wealth-driven.
- Modeling (% change):
  - Ridge regression using `log(FY20)`, `log(FTE)`, per-FTE, `Type`, `Country`, `Carnegie` yields modest explanatory power (R² in low range), suggesting FY21 performance was influenced by factors outside these static institution features (e.g., asset allocation, flows, policy decisions).

## Models and outputs
- Correlation coefficients (as observed in the notebook):
  - FTE vs FY21 Endowment: Pearson r = 0.385 (p = 2.24e-22), Spearman ρ = 0.472 (p = 2.65e-34)
  - log(FY20 Endowment) vs % Change: Pearson r = 0.187 (p = 3.33e-07), Spearman ρ = 0.321 (p = 4.97e-19)
- Regression (% Change target):
  - Model: Ridge regression with preprocessing (median imputation + standardization for numeric; most-frequent imputation + one-hot for categorical `Type`, `Country`, `Carnegie`).
  - Outputs: The regression cell in `data.ipynb` prints R² (test), MAE (pct-pts), and 5-fold CV R² (mean±sd). Run that cell to view exact values for your environment/filters.
- Clustering (institution segments):
  - Model: K-Means (k=4) on features `[log(FTE), log(FY21_Endowment_$), FY21_Per_FTE_$, Pct_Change]` with standardization.
  - Outputs: The clustering cell prints the silhouette score and shows two scatterplots with cluster labels.

##Based on the output, the United States institutions collectively hold significantly larger endowment market values compared to Canadian ones.

Scale and Donor Culture:
U.S. universities have much larger alumni networks and strong traditions of private donations and endowment management.
(e.g., Harvard alone has $52B+  more than all Canadian universities combined.)

Tax and Philanthropy Laws:
The U.S. offers stronger tax incentives for charitable giving to universities.

Historical Investment Structures:
American endowments have existed for centuries and compound over long periods; Canadian endowments are relatively younger.

Currency & Economic Factors:
Endowments are reported in USD, and exchange rate fluctuations can make Canadian totals appear smaller.