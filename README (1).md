# Adaptive Market Hypothesis: US vs Mexico

Code and analysis for "The comparison of AMH in Mexican and U.S. markets," a study
comparing time-varying market efficiency in the US (S&P 500) and Mexican (BMV) stock
markets from January 2000 to March 2026.

## Overview

The analysis tests the Adaptive Market Hypothesis using four statistical tests
(Variance Ratio, Ljung-Box, BDS, and Automatic Portmanteau) applied in both static
full-period and rolling-window designs, with GARCH(1,1) filtering to separate genuine
return predictability from volatility clustering. A stock-level heterogeneity analysis
groups stocks by sector, market capitalization, and liquidity.

## Files

- `AMH_project.ipynb` — main notebook: data download, preprocessing, static and
  rolling analyses, and cross-test correlations
- `heterogeneity_full.py` — computes per-stock rolling rejection rates and fetches
  sector, market cap, and dollar volume for each stock
- `heterogeneity_stats.py` — Kruskal-Wallis and Mann-Whitney U tests across sector,
  market cap, and liquidity groups
- `boxplot_figures.py` — generates the heterogeneity boxplots (Figures 6-8)

## Data

All price data is downloaded from Yahoo Finance via the `yfinance` library, so no data
files are included. The BMV ticker universe is based on the dataset compiled by
Trevino Aguilar (2020).

## Requirements

Python 3.10+ and R (for the vrtest package, called via rpy2).

Install Python dependencies:

    pip install numpy pandas matplotlib scipy statsmodels arch yfinance rpy2

Install the R package (from an R console):

    install.packages("vrtest")

## How to run

1. Run `AMH_project.ipynb` for the static, rolling, and correlation results.
2. Run `heterogeneity_full.py` to produce the per-stock CSVs (takes 30-40 minutes).
3. Run `heterogeneity_stats.py` for the group tests.
4. Run `boxplot_figures.py` for the heterogeneity figures.

## Author

Emiliano Gonzalez Tijerina — [your email]

## Citation

If referencing this work: [paper citation once published]
