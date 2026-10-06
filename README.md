# Dividend Payers vs Non-Payers: S&P 500 Return and Risk Backtest

A Python backtest that splits the S&P 500 into dividend payers and non-payers, builds an equal-weight portfolio of each, and compares their return, risk, and drawdown from January 2018 to September 2025. Prices come from Yahoo Finance.

In this backtest the non-dividend portfolio did better. It returned 22.1% a year against 15.0% for the dividend payers, with higher volatility (24.6% against 19.8%), and its Sharpe ratio was higher too (0.90 against 0.76). The result carries two biases that favor the non-payers, covered in Limits.

## Key results

| | Dividend payers | Non-dividend |
|---|---|---|
| Stocks in portfolio | 401 | 91 |
| Annual return | 15.04% | 22.10% |
| Volatility | 19.77% | 24.59% |
| Sharpe ratio (risk-free rate of 0) | 0.76 | 0.90 |
| Maximum drawdown | -38.65% | -40.39% |
| Total return | 173.45% | 333.41% |

The maximum drawdowns are close. The non-payers took more risk, and the Sharpe ratios show they were paid for it over this window.

## How it works

1. **Universe.** The current S&P 500 list (503 tickers) is pulled from Slickcharts, with Wikipedia as a backup.
2. **Grouping.** Each stock goes into the payer group if its current dividend yield is above zero, and into the non-payer group if it is exactly zero. This gave 407 payers and 96 non-payers.
3. **Prices.** Adjusted daily closing prices from January 1, 2018 are downloaded in batches of 20 with retries. Stocks with price history for less than 70% of the period are dropped, which leaves 401 payers and 91 non-payers.
4. **Portfolios.** Each group is an equal-weight portfolio, which is the average of the daily returns of its stocks.
5. **Metrics.** Annual return, volatility, Sharpe ratio, maximum drawdown, and total return for each portfolio, plus a growth of $1 chart and a risk and return bar chart.

## Limits

- **Groups use today's dividend yield.** A stock is sorted using its yield at the time of the run and then tracked back to 2018. A company that started paying a dividend after 2018 sits in the payer group for years when it paid nothing, and the reverse also happens. A stricter test would re-form the groups each December using that year's yield and measure the next year's return.
- **Survivorship.** The universe is today's S&P 500, so companies that were removed or went bankrupt are not in the sample. Stocks that grew large enough to join the index are over-represented, which tends to favor the faster growing non-payers.
- **Equal weight, no costs.** Both portfolios are averaged each day, which implies daily rebalancing with no trading costs or taxes.
- **Return definition.** Annual return is the average daily return times 252, and total return compounds the daily returns. The Sharpe ratio uses a risk-free rate of 0.
- **Moving window.** The end date is the day the notebook is run, so rerunning it gives different numbers. The results above are from September 21, 2025.

## Files

- `Dividend_Payers_vs_Non_Payers_SP500_Backtest.ipynb`: the full notebook, with code and outputs

## Running it

Run the notebook in Jupyter or Google Colab with an internet connection. It needs `yfinance`, `pandas`, `numpy`, `matplotlib`, `requests`, and `lxml`. The dividend snapshot takes a few minutes because it queries one stock at a time.

This is a student project and not investment advice.
