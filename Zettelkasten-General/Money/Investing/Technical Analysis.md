---
tags:
  - Money
  - Investing
---
# Moving Averages (e.g., 50-day & 200-day)

Moving averages smooth out price fluctuations and help traders identify trends.

50-day Moving Average (MA): Shorter-term trend indicator; reacts faster to price changes.

200-day Moving Average (MA): Long-term trend indicator; helps determine overall market direction.

Golden Cross: When the 50-day MA crosses above the 200-day MA → Bullish signal.

Death Cross: When the 50-day MA crosses below the 200-day MA → Bearish signal.
Example: In 2020, Apple’s stock showed a Golden Cross before a major price rally.


# Relative Strength Index (RSI)

RSI measures momentum and determines whether a stock is overbought or oversold (scale: 0-100).

Above 70: Overbought → Potential reversal or pullback.

Below 30: Oversold → Potential buying opportunity.
Example: Tesla (TSLA) had an RSI of 85+ in late 2021 before a pullback, signaling a temporary overbought condition.


# MACD (Moving Average Convergence Divergence)

MACD helps traders spot trend changes. It consists of:

MACD Line (12-day EMA - 26-day EMA)

Signal Line (9-day EMA of MACD Line)

Histogram (Difference between MACD and Signal Line)

Bullish Signal: MACD crosses above Signal Line → Uptrend.

Bearish Signal: MACD crosses below Signal Line → Downtrend.
Example: Amazon’s stock in mid-2022 saw a MACD crossover before a sharp rally.


# Support & Resistance Levels

Support: A price level where a stock tends to stop falling due to buying interest.

Resistance: A price level where a stock tends to stop rising due to selling pressure.

Breakout Strategy: If a stock breaks above resistance, it may continue rising.
Example: S&P 500’s 4,200 level was a key resistance in 2023 before a breakout led to a sustained uptrend.


How These Indicators Work Together

1. RSI + MACD: If RSI is oversold and MACD shows a bullish crossover → Stronger buy signal.
2. Moving Averages + Support/Resistance: A stock breaking above the 200-day MA and a key resistance level → Strong bullish confirmation.
3. RSI + Moving Averages: If RSI is overbought but the stock is still above the 50-day MA → Possible continuation of the uptrend.

# Simple Moving Averages (SMA)

A Simple Moving Average (SMA) is a commonly used technical indicator in stock trading that helps smooth out price data to identify trends over a specific period. It is calculated by taking the average closing price of a stock over a set number of days.

Formula for SMA:

$$
SMA = \frac{P_1 + P_2 + ... + P_n}{n}
$$
 are the closing prices for each day in the period

 is the number of days in the moving average


Example Calculation:

For a 5-day SMA, if the last five closing prices are 10, 12, 14, 16, and 18, the SMA would be:
$$
SMA = \frac{10 + 12 + 14 + 16 + 18}{5} = \frac{70}{5} = 14
$$

How SMA is Used:

1. Trend Identification: A rising SMA suggests an uptrend, while a falling SMA suggests a downtrend.

2. Support & Resistance Levels: Prices often bounce off SMAs, making them useful for predicting potential support and resistance areas.

3. Crossovers:
	- A Golden Cross (short-term SMA crosses above long-term SMA) signals a potential uptrend.
	- A Death Cross (short-term SMA crosses below long-term SMA) signals a potential downtrend.

Common SMA Periods:
- Short-term: 10-day, 20-day (used for quick trend changes)
- Medium-term: 50-day (popular among traders)
- Long-term: 200-day (used for overall market trends)

# Exponential Moving Average (EMA)
Yes! An Exponential Moving Average (EMA) is a type of moving average that gives more weight to recent prices, making it more responsive to price changes compared to a Simple Moving Average (SMA).

Formula for EMA:

$$
EMA = ( \text{Current Price} \times \text{Multiplier} ) + ( \text{Previous EMA} \times (1 - \text{Multiplier}) )
$$

Multiplier (also called smoothing factor) is calculated as:
$$
\frac{2}{n+1}
$$

The Previous EMA starts as an SMA for the first calculation.


Example Calculation (10-day EMA):

1. Calculate the SMA for the first 10 days (this acts as the first EMA value).

2. Compute the multiplier:

$$
\frac{2}{10+1} = \frac{2}{11} \approx 0.1818
$$

If today's price is 50 and yesterday’s EMA was 48, the new EMA is:

$$
(50 \times 0.1818) + (48 \times 0.8182) = 48.36
$$

Why Use EMA?

More Responsive to Trends – EMA reacts faster to price changes than SMA.

Better for Short-Term Trading – Traders use it for momentum strategies.

Works Well for Crossovers – Many traders use 50-day EMA & 200-day EMA for signals.


Common EMA Periods:

- Short-term: 10-day, 20-day (quick reactions)
- Medium-term: 50-day (trend confirmation)
- Long-term: 200-day (overall trend)

