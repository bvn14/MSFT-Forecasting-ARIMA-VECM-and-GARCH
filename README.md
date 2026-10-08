# MSFT Forecasting: ARIMA, VECM and GARCH

Time series econometrics project (DA4421).Model Microsoft (MSFT) daily closing prices from April 2015 to April 2026. The S&P 500 and the VIX are used as extra variables in the multivariate model.

## Methods

1. **Univariate model.** SARIMA(1,1,1)(0,0,1)[5] on daily prices. Steps: ADF and KPSS tests, ACF and PACF plots, `auto_arima`, Ljung-Box residual tests, 30-day forecast.
2. **Multivariate model.** VECM with MSFT, SP500 and VIX. Steps: unit root tests, Johansen cointegration test, lag selection, residual checks, impulse response functions, 30-day forecast.
3. **Volatility model.** ARCH-LM tests on the residuals of both models, then GARCH(1,1) with Student-t errors, compared with EGARCH and GJR-GARCH.

## Key results

| Item | Result |
|---|---|
| Order of integration | MSFT is I(1). One difference gives a stationary series. |
| Best ARIMA by AIC | SARIMA(1,1,1)(0,0,1)[5] |
| Cointegration | Johansen test finds one cointegrating relation (r = 1) |
| VECM lag order | 8 lagged differences (removes residual autocorrelation) |
| 30-day hold-out RMSE (MSFT) | ARIMA 24.57, VECM 24.59 |
| ARCH effects | Present in both models (ARCH-LM p < 0.001) |
| Best GARCH variant by AIC | GARCH(1,1) with Student-t errors |
| After GARCH | No ARCH effects left in standardised residuals (p > 0.5) |

The extra variables did not improve the point forecast. Both models give a nearly flat forecast, which is common for stock prices.

## Limitations

- The GARCH persistence (alpha + beta) is about 1, so volatility shocks last very long and the volatility forecast is almost flat.
- VIX looks stationary in levels (ADF test), so the cointegrating relation is mostly driven by VIX.
- The hold-out test uses one 30-day window, so the error estimates are not very stable.

## How to run

```
pip install -r requirements.txt
jupyter notebook notebooks/msft_arima_vecm_garch.ipynb
```

## Data

Daily closing prices from Yahoo Finance using `yfinance` (tickers: MSFT, ^GSPC, ^VIX). A saved copy is in `data/MSFT_Multivariate_Data.csv`, so the results can be reproduced even if Yahoo data changes.

## Tools

Python, pandas, numpy, statsmodels, pmdarima, arch, scikit-learn, matplotlib, seaborn, yfinance
