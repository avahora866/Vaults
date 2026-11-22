---
tags:
  - Money
  - Investing
  - LEAF
---
# Relative Strength Index (RSI)

## What is RSI?

The **Relative Strength Index (RSI)** is a momentum oscillator that measures the speed and change of price movements. It was developed by **J. Welles Wilder** in the late 1970s. The RSI provides insight into whether an asset is **overbought** or **oversold**, helping traders identify potential price reversals.

## RSI Formula:

RSI=100−(1001+RS)RSI \= 100 \- \\left( \\frac{100}{1 \+ RS} \\right)

Where:

* **RS** (Relative Strength) is the ratio of **average gains** to **average losses** over a specified period, typically 14 periods.
* **Average Gain** \= sum of gains over the period ÷ 14
* **Average Loss** \= sum of losses over the period ÷ 14

## RSI Calculation Steps:

1. **Calculate average gain and loss** over the 14-period.
2. **Calculate RS**: the ratio of average gain to average loss.
3. **Apply the formula** to compute the RSI value.

## RSI Range:

* The RSI value ranges from **0 to 100**.
	* *Overbought**: RSI \> 70 (signals that the asset may be overbought and could see a price pullback)
	  * **Oversold**: RSI \< 30 (signals that the asset may be oversold and could see a price bounce)
	  * **Neutral zone**: RSI between 30 and 70 is considered neutral, indicating that the asset is in a healthy trend.

## How RSI is Used in Trading:

* **Overbought/Oversold Conditions**:
	- When the RSI crosses above **70**, the asset may be **overbought**, indicating a potential **sell signal**.
	  * When the RSI crosses below **30**, the asset may be **oversold**, indicating a potential **buy signal**.
* **Divergences**:
	* A divergence occurs when the price moves in the opposite direction of the RSI. For example:
		* If prices are making new highs but the RSI is not, it could signal a **bearish divergence** and a possible price reversal.
		* If prices are making new lows but the RSI is not, it could signal a **bullish divergence** and a potential trend reversal to the upside.
* **Centerline Crossovers**:
- When the RSI crosses the **50 level**, it can indicate a shift in momentum:
	- If the RSI crosses **above 50**, it could signal bullish momentum.
	- If the RSI crosses **below 50**, it could signal bearish momentum.

## RSI Example in Practice:

If a stock is trading at $100 and experiences a strong rally up to $120, the RSI might rise toward the overbought level of **70** or higher. Traders might see this as an indication to **sell** or look for a price pullback. Conversely, if the stock falls to $80 and RSI drops below **30**, traders might consider a **buy** signal, expecting a price rebound.

---

# Average Directional Index (ADX)

## What is ADX?

The **Average Directional Index (ADX)** is an indicator used to quantify the **strength of a trend**. Unlike the RSI, which measures the momentum or overbought/oversold conditions, the ADX focuses specifically on the **trend strength**, whether the trend is up or down. It was also developed by **J. Welles Wilder**.

The ADX is typically used in conjunction with two other indicators:

* **\+DI (Positive Directional Indicator)**: Measures the strength of upward price movements.
* **\-DI (Negative Directional Indicator)**: Measures the strength of downward price movements.

Together, these indicators help determine not only the **strength** but also the **direction** of the trend.

## ADX Formula:

The ADX is calculated using a complex smoothing process involving the **\+DI** and **\-DI** lines. While the exact formula is beyond the typical needs for basic usage, here's an overview of its steps:

1. **Calculate the directional movement** (DI) values for each period (positive and negative).
2. **Smooth the values** using a moving average over a set period, typically 14 periods.
3. **Calculate the ADX** by averaging the differences between \+DI and \-DI over time.

## ADX Value Range:

* The ADX value ranges from **0 to 100**.
	- **0 to 20**: Indicates a **weak trend** or no trend.
	- **20 to 40**: Indicates a **moderate trend**.
	- *40 to 60**: Indicates a **strong trend**.
	- *60 to 100**: Indicates an **extremely strong trend** (often unsustainable).

## How ADX is Used in Trading:

- **Trend Strength**:
	- An ADX value **above 20** suggests that there is a trend (either up or down).
	- An ADX value **below 20** suggests a **range-bound market** (no clear trend).
* **Trend Direction**:
	* The **\+DI** and **\-DI** lines are used to identify whether the trend is **up** or **down**:
	    * When **\+DI \> \-DI**, it signals a **bullish trend**.
	    * When **\-DI \> \+DI**, it signals a **bearish trend**.
* **Combining ADX with \+DI and \-DI**:
	* The **ADX** value shows the **strength of the trend**, while the **\+DI** and **\-DI** lines show the **direction** of the trend.
	* Traders often use the **ADX** to avoid trading in low-volatility, range-bound markets (ADX below 20\) and focus on **strong trends** (ADX above 20).

## ADX Example in Practice:

* **Bullish Trend**: If the **\+DI** is above the **\-DI** line and the **ADX** is rising above 20, it signals a **strong upward trend**.
* **Bearish Trend**: If the **\-DI** is above the **\+DI** line and the **ADX** is rising above 20, it signals a **strong downward trend**.
* If the **ADX** is falling and both **\+DI** and **\-DI** are close to each other, it suggests that the market is in a **weak or neutral phase** with no clear trend.

---

# RSI vs. ADX

| Feature | RSI | ADX |
| ----- | ----- | ----- |
| **Purpose** | Measures momentum and overbought/oversold conditions | Measures trend strength, not direction |
| **Range** | 0–100 (Overbought \> 70, Oversold \< 30\) | 0–100 (Trend strength) |
| **Primary Use** | Identify potential price reversals | Confirm strength of a trend (up or down) |
| **Signals** | Overbought/oversold conditions, divergences, centerline crossovers | Trend strength and trend direction (through \+DI and \-DI) |

---

# Using RSI and ADX Together

Many traders use both **RSI** and **ADX** together for more robust signals:

* **RSI** can be used to identify **overbought or oversold conditions**.
* **ADX** can confirm whether the market is in a **strong trend** (ADX \> 20\) or in a **range** (ADX \< 20).

## Example Combination:

* A **buy signal** could be triggered when the RSI shows **oversold conditions (RSI \< 30\)**, while ADX indicates a **strong uptrend (ADX \> 20, \+DI \> \-DI)**.
* A **sell signal** could be triggered when the RSI shows **overbought conditions (RSI \> 70\)**, while ADX indicates a **strong downtrend (ADX \> 20, \-DI \> \+DI)**.

---

# Summary

* **RSI** is a momentum oscillator that helps identify overbought and oversold conditions, which could indicate a potential price reversal.
* **ADX** measures the strength of a trend and is used to determine whether a market is trending or ranging.
* **RSI** focuses on price momentum, while **ADX** focuses on trend strength.
* These indicators are often used together to confirm signals and ensure trades are made in strong trending conditions
