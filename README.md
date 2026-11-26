# Bitcoin Price Forecasting with ARIMA

This project analyzes daily Bitcoin closing prices in USD and evaluates one-day-ahead forecasts with ARIMA. I used the BTC-USD series from Yahoo Finance via `yfinance`. The saved run covers 3,905 daily observations from September 17, 2014, through May 26, 2025.

## Method

1. Split prices chronologically into 3,124 training days and 781 test days.
2. Explore trend and seasonality with additive and multiplicative decomposition. Examine log prices and first differences using the Augmented Dickey–Fuller test and ACF/PACF plots.
3. Compare candidate ARIMA orders using AIC and BIC and check residual autocorrelation with the Ljung–Box test.
4. Backtest an ARIMA(0, 1, 1) model on closing prices. Forecast one day ahead, add the observed closing price to the training window, and refit before forecasting the next day.

## Results and limitations

- The saved backtest produced **1.76% mean absolute percentage error (MAPE)** over the 781 test days.
- The Ljung–Box test at lag 10 detected residual autocorrelation (p ≈ 0.000012). A last-close baseline would help determine whether the ARIMA model improves on a simple one-day-ahead forecast.
- The order comparison applies ARIMA with `d=1` to an already differenced log series. The backtest applies ARIMA with `d=1` directly to price levels. The candidate information criteria and the reported forecast error therefore refer to different model specifications.

## Run locally

The notebook was saved with Python 3.10. Install the dependencies and open it from the repository root.

```bash
python -m pip install -r requirements.txt
jupyter notebook time_series_project.ipynb
```

The notebook downloads prices at runtime, so an undated download can produce a different split and MAPE. For the saved date range, set `start="2014-09-17"` and `end="2025-05-27"` in the download call.

The final section explores monthly averages with Auto ARIMA. To run that section from a fresh kernel, add `from pmdarima import auto_arima` before the call. For pandas versions that reject the `M` alias, use `pd.offsets.MonthEnd()` in the monthly resampling call.

The [original notebook](time_series_project.ipynb) includes the saved charts, diagnostic tests, and forecast results.
