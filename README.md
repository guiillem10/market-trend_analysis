# Market Trend Analysis and Investment Recommendation Toolkit

This project is a comprehensive Python library and command‑line tool
that brings together several core techniques from quantitative finance
to analyse historical market data, forecast future price movements and
produce simple investment recommendations. It is designed as an
educational showcase for students and practitioners who wish to see
stochastic processes, time‑series models and option pricing theory
combined into a coherent workflow. **Nothing in this repository
constitutes financial advice; it is for demonstration purposes only.**

## Background

Financial asset prices are often modelled as geometric Brownian
motion (GBM). Under GBM the percentage change of the asset price
follows a stochastic differential equation that implies a log‑normal
distribution for future prices. This is the same
assumption used by the Black–Scholes option pricing model.
The Black–Scholes formula computes the theoretical value of a European
call option by subtracting the discounted strike price weighted by the
cumulative normal distribution from the current stock price weighted
by another cumulative normal distribution【367345182495073†L462-L473】. The inputs
required are the current price, strike price, risk‑free rate, time to
maturity and volatility.

When analysing historical price series it is common to compute
log‑returns, which are the natural logarithm of price ratios. These
returns are often modelled by ARIMA processes. An ARIMA
model is one of the most general classes of forecasting models for
series that have been made stationary by differencing; a stationary
series has constant mean and variance and unchanging autocorrelations. The ARIMA forecasting equation expresses the
future value of the series as a linear combination of its past values
and past forecast errors. The acronym ARIMA
stands for AutoRegressive Integrated Moving Average, and its three
parameters `(p, d, q)` denote respectively the number of
autoregressive terms, the number of differences required for
stationarity and the number of moving‑average terms.

## Features

- **Data retrieval and preprocessing** – Pulls historical OHLCV data
  from Yahoo! Finance via `yfinance` and computes log‑returns.
- **ARIMA forecasting** – Fits an ARIMA model to historical returns
  (optionally performing a simple grid search to choose the order) and
  forecasts future returns. The library uses the `statsmodels` ARIMA
  implementation.
- **Stochastic simulation** – Estimates drift and volatility from
  historical returns and simulates future price paths under geometric
  Brownian motion, providing a distributional view of possible
  outcomes.
- **Option pricing and Greeks** – Prices at‑the‑money European call and
  put options using the Black–Scholes formula and computes the
  standard sensitivities (Delta, Gamma, Rho, Theta and Vega).
- **Recommendation engine** – Combines the forecasted mean and
  percentiles of the simulated distribution with a simple
  threshold‑based rule to issue a “BUY”, “SELL” or “HOLD” signal.
- **Command‑line interface** – The `main.py` script can analyse one or
  more ticker symbols and save results to JSON.

## Installation

Clone this repository and install the dependencies. It is
recommended to use a virtual environment:

```bash
git clone <your-fork-url>
cd market_trend_analysis
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

The key dependencies are:

- `yfinance` for downloading historical data
- `pandas` and `numpy` for data handling
- `statsmodels` for ARIMA modelling
- `matplotlib` (optional) for plotting
- `scipy` for option pricing routines

## Usage

To run the analysis for one or more tickers, execute the main script
from the project root:

```bash
python -m market_trend_analysis.src.main AAPL MSFT --horizon 30 --paths 2000 --auto-arima --output results.json
```

This will:

1. Download the past three years of daily data for `AAPL` and
   `MSFT`.
2. Fit an ARIMA model to the log returns and forecast 30 trading days
   ahead.
3. Simulate 2,000 geometric Brownian motion paths over the same
   horizon.
4. Price at‑the‑money European call and put options with maturity
   `30/252` years and risk‑free rate 2 %.
5. Compute the Greeks of the call option.
6. Generate a simple recommendation based on the forecasted mean and
   simulated distribution.
7. Save the full results as JSON to `results.json`.

The console will display a human‑readable summary for each ticker.

## Limitations and Further Work

- The ARIMA model selection is rudimentary: it performs a small grid
  search over `(p, d, q)` based on AIC. More sophisticated methods
  (e.g. `pmdarima.auto_arima`) or models (e.g. seasonal ARIMA,
  GARCH, Prophet) could be incorporated.
- The geometric Brownian motion model assumes constant drift and
  volatility and log‑normal price dynamics【367345182495073†L482-L491】. Real
  markets exhibit stochastic volatility, jumps and other features
  ignored here.
- The Black–Scholes formula only prices European options and assumes
  constant volatility【367345182495073†L569-L583】. Extensions such as
  implied‑volatility surfaces or the Heston model could enhance
  realism.
- The recommendation engine is deliberately simple and should not be
  used for trading decisions. It does not account for transaction
  costs, liquidity, portfolio context or risk tolerance.

Despite these limitations, the project demonstrates how classic
techniques from quantitative finance can be integrated into a single
analysis pipeline.
