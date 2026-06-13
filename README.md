# Adaptive Market Hypothesis: A Comparative Analysis of US (S&P 500) and Mexican (BMV) Stock Markets

This repository contains the code, data outputs, and analysis for a research project comparing time-varying market efficiency in the US and Mexican stock markets under the Adaptive Market Hypothesis (AMH) framework, covering the period January 2000 to March 2026.

## Overview

The study applies four statistical tests for weak-form market efficiency — the Variance Ratio test, Ljung-Box test, BDS test, and Automatic Portmanteau test — to 40 S&P 500 stocks and 54 Bolsa Mexicana de Valores (BMV) stocks. Tests are run in both static (full-period) and rolling window designs, with GARCH(1,1) filtering applied to separate volatility clustering from genuine return predictability.

**Key findings:**
- Both markets show time-varying efficiency consistent with the AMH
- US inefficiency is crisis-driven (GFC, COVID) and temporary
- Mexican inefficiency is structural and persistent across the full sample period
- Mexico shows approximately double the genuine return predictability of the US after controlling for volatility effects

## Repository Structure

```
AMH-research/
├── AMH_project.ipynb          # Main analysis notebook (Google Colab)
├── README.md                  # This file
├── requirements.txt           # Python package dependencies
├── data/
│   ├── 00clavesBMV.csv       # Validated BMV ticker list (Treviño Aguilar 2020)
│   └── sp500_constituents.csv # S&P 500 constituent list as of January 2000
├── results/
│   ├── static/               # Full-period test results per stock
│   ├── rolling/              # Rolling window time series (250-day, 21-step)
│   └── robustness/           # 500-day window robustness check
└── figures/                  # All plots used in the paper
```

## Requirements

**Python (3.10+):**
```
arch >= 6.0
statsmodels >= 0.14
pandas >= 2.0
numpy >= 1.24
matplotlib >= 3.7
yfinance >= 0.2
rpy2 >= 3.5
```

**R (4.0+):**
```
vrtest
```

Install Python dependencies:
```bash
pip install -r requirements.txt
```

Install R package from within Python:
```python
import rpy2.robjects as ro
ro.r('install.packages("vrtest", repos="https://cloud.r-project.org")')
```

## How to Run

The full analysis is contained in `AMH_project.ipynb`. To reproduce the results:

1. Open the notebook in Google Colab or Jupyter
2. Mount Google Drive if running in Colab and ensure the `data/` folder is accessible
3. Run cells in order from top to bottom

The pipeline performs:
- Data download from Yahoo Finance via `yfinance`
- Log return computation and coverage filtering (≥80%)
- GARCH(1,1) filtering for tests requiring standardized residuals
- Static analysis of all four tests on the full sample period
- Rolling window analysis (250-day window, 21-day step) for all tests
- Robustness check with 500-day window, 63-day step for VR
- Cross-test Pearson correlation analysis on rolling rejection rates
- Generation of all figures used in the paper

**Estimated runtime:** approximately 45–60 minutes on Google Colab (free tier). The rolling AP test via the R `vrtest` package is the most time-intensive component.

## Data Sources

- Daily adjusted closing prices: Yahoo Finance via the `yfinance` Python library
- US stock universe: historical S&P 500 constituent list as of January 2000
- Mexican stock universe: validated BMV ticker list compiled by Treviño Aguilar (2020), DOI: [10.1371/journal.pone.0238731](https://doi.org/10.1371/journal.pone.0238731)

All raw data is reconstructable through the notebook — no proprietary datasets are required.

## Statistical Tests

| Test | Purpose | Implementation |
|------|---------|----------------|
| Variance Ratio | Linear predictability across multiple horizons | `arch.unitroot.VarianceRatio` |
| Ljung-Box | Linear autocorrelation at specific lags | `statsmodels.stats.diagnostic.acorr_ljungbox` |
| BDS | Nonlinear dependence in GARCH residuals | `statsmodels.tsa.stattools.bds` |
| Automatic Portmanteau | Autocorrelation with automatic lag selection | `vrtest::Auto.Q` (R) |
| GARCH(1,1) | Volatility filtering | `arch.arch_model` |

## Reproducibility

All methodological choices were fixed before observing results:
- Random seed: 42 (primary US sample), 99 (top-up procedure)
- Rolling window: 250 days, 21-day step
- VR lags: [2, 5, 22, 66, 130]
- LB lags: [5, 10, 20]
- BDS max dimension: 6
- Significance threshold: 5%
- Coverage threshold: 80% (sample), 70% (within rolling window)

## Citation

If you use this code or data in your work, please cite the paper:

> Gonzalez, E. (2026). The comparison of AMH in Mexican and U.S. markets. *[Journal name pending]*.

## License

Code is released under the MIT License. Data is publicly available through Yahoo Finance.

## Contact

Emiliano Gonzalez Tijerina — emigleztije@gmail.com  
Texas Military Institute, San Antonio, TX

## Acknowledgements

Project conducted as part of the Aspire Research Fellowship under the mentorship of Nathan Geldner.
