# Portfolio Trading Strategy Backtester
This Python script implements and backtests a multi-asset trading strategy on a portfolio of several different stock tickers. It leverages common technical indicators to generate buy and sell signals and simulates portfolio performance over historical data.

---

## Core Functionality
* Multi-Asset Backtesting: The script is designed to handle a portfolio of several user-defined ticker symbols. It concurrently downloads, processes, and backtests the strategy on each asset, aggregating the results to track the total portfolio equity.
* Indicator-Based Signals: The trading logic is built on a dual-indicator system:
  * 200-Day Simple Moving Average (SMA): The script calculates the 200-day SMA, a long-term trend-following indicator. A buy signal is generated only when the closing price of the asset is above this moving average, confirming an uptrend.
  * 2-Period Relative Strength Index (RSI): The 2-period RSI, a momentum oscillator, is used to identify short-term overbought or oversold conditions. A buy signal is triggered when the RSI falls below a threshold of 10, indicating that the asset may be oversold.
* Trading Logic:
  * Buy Signal: A position is initiated when both the closing price is greater than the 200-day SMA and the 2-period RSI is less than 10. The script allocates an equal portion of the initial capital to each ticker.
  * Sell Signal: A position is exited when the current day's closing price exceeds the previous day's high, indicating a strong upward move that could be a temporary spike.
* Data Handling: The yfinance library is used for downloading historical stock data from Yahoo Finance. The script gracefully handles tickers for which no data is available and aligns the backtesting period across all assets to ensure a consistent simulation.
* Performance Visualization:
  * A two-pane matplotlib plot is generated to visualize the backtesting results.
  * The top pane displays the closing prices and 200-day SMA for each ticker, with distinct markers for buy and sell signals.
  * The bottom pane shows the cumulative portfolio equity growth on a logarithmic scale, making it easier to visualize percentage changes over time.

## Technical Breakdown
The backtesting engine operates on a synchronized timeline, ensuring all tickers in the portfolio are evaluated on the same dates. This is achieved by first creating a consolidated, unique, and sorted list of all trading dates across all downloaded datasets using pd.concat(). A minimum start date for the backtest is then determined as the date after which the 200-day SMA is available for all tickers, preventing data look-back errors.

The core of the simulation is a for loop that iterates through this master list of dates. For each day, the script uses a dictionary (portfolio_state) to maintain the independent state of each ticker (e.g., in_position, shares, cash, entry_dates, exit_dates). This enables the trading logic to be applied individually to each asset, allowing it to make autonomous buy/sell decisions based on its own data. The portfolio's total value is then calculated by summing the cash and current market value of all holdings on that day.

For performance visualization, itertools.cycle is used to efficiently assign a different marker style and color to the buy/sell signals of each ticker, enhancing the readability of the multi-asset plot without requiring manual assignment.

## Performance Metrics Explained
This Python function: 
```python
def calculate_metrics(equity_curve, initial_capital, risk_free_rate=0.04):
```
is used to evaluate the performance of a trading strategy. By analyzing an equity curve—a time series representing the strategy's total portfolio value—it computes several essential metrics that provide a comprehensive view of the strategy's profitability, risk, and consistency.

**1. Total Portfolio Return:** The Total Return measures the overall profitability of the strategy over the entire backtested period. It's calculated as the percentage change from the initial capital to the final portfolio value. A positive return indicates a profit, while a negative return signifies a loss. This metric is useful for understanding the absolute growth of the portfolio.

**2. Compounded Annual Growth Rate (CAGR):** CAGR represents the average annual return that an investment needs to generate to grow from its initial balance to its final balance, assuming the profits are reinvested each year. Unlike the total return, which provides a single snapshot, CAGR provides a smoothed, annualized rate of return, making it an excellent tool for comparing the performance of different strategies over varying time periods.

**3. Maximum Drawdown:** Maximum Drawdown (MDD) is a key risk metric that measures the largest peak-to-trough decline in the portfolio's value over a specific period.  It's expressed as a percentage and indicates the worst-case scenario loss an investor would have endured if they had bought at a peak and sold at the subsequent trough. A lower MDD signifies a more resilient strategy with less capital risk.

**4. Sharpe Ratio:** The Sharpe Ratio measures the risk-adjusted return of an investment. It tells you how much excess return you are getting for the extra volatility you endure. It's calculated by subtracting the risk-free rate from the strategy's return and dividing the result by the standard deviation of its returns. A higher Sharpe Ratio indicates a better return for a given level of risk, meaning the strategy is generating returns more efficiently.

**5. Sortino Ratio:** Similar to the Sharpe Ratio, the Sortino Ratio also measures risk-adjusted return but focuses specifically on downside volatility. It penalizes only the negative returns (or "bad" volatility), as it's concerned with how much return a strategy is generating relative to the risk of significant losses. This makes it particularly useful for strategies that aim to minimize losses, as it ignores the volatility from positive price movements. A higher Sortino Ratio suggests better performance for a given level of downside risk.

## Installation & Usage
Clone the repository:
```bash
    git clone https://github.com/handiko/RSI-2-Portfolio-Trading-Strategy-Backtester.git
```

Navigate to the directory:
```bash
    cd RSI-2-Portfolio-Trading-Strategy-Backtester.
```

Install the required libraries:
```bash
    pip install -r requirements.txt (assuming a requirements.txt file exists) or manually:
    pip install yfinance pandas matplotlib numpy
```

Modify the ticker_symbols list in main() to your desired tickers.
Run the script:
```bash
    python your_script_name.py
```

Code: [here](https://github.com/handiko/RSI-2-Portfolio-Trading-Strategy-Backtester/blob/main/JupyterNotebook/Portfolio%20Mode%20-%202-RSI%20Trading%20Strategy.ipynb)

## Example
### Backtesting A Portfolio Consisting of 3 Indonesian stocks (BBCA, BMRI, and TLKM)
![](./portfolio_trading_strategy_plot_3_tickers.png)



### Backtesting A Portfolio Consisting of Meta and Microsoft (META & MSFT)
![](./portfolio_trading_strategy_plot_2_tickers.png)



### Backtesting the FAANG+ Portfolio (META, AAPL, AMZN, NVDA, GOOG, & MSFT)
![](./portfolio_trading_strategy_plot_6_tickers.png)

---

## Disclaimer!!
This repository is intended for educational purposes only and does not constitute financial advice. Past performance is not indicative of future results.

---

## Related Project
* [RSI-2 Stock Strading Strategy](https://github.com/handiko/RSI-2-Stock-Trading-Strategy/blob/main/README.md)
* [RSI-2 Trading Strategy - Pinescript version](https://github.com/handiko/RSI-2-Stock-Trading-Strategy-Pinescript/blob/main/README.md)

---

Back to [Index](https://github.com/handiko/handiko/blob/master/README.md)
