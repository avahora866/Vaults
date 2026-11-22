### Phase 1: Core Economic Principles

This phase builds the foundation. You must understand *how* economic agents behave before you can model them.

* **Microeconomics (The "Bottom-Up" View)**
    * **Supply and Demand:** This is the core mechanism of all markets. Understand how price is determined.
    * **Elasticity:** Measures sensitivity. How much does demand for a stock (or any good) change if its price moves 1%?
    * **Consumer Utility Theory:** Understand the *why* behind a decision. This is the basis for modeling risk aversion, which is central to all finance.
    * **Firm Behavior:** Production costs, and market structures (monopoly, perfect competition). This helps in analyzing company valuations.

* **Macroeconomics (The "Top-Down" View)**
    * **Key Indicators:** GDP (growth), inflation (CPI), and unemployment. These variables move entire markets.
    * **Monetary Policy:** Focus heavily here. Understand how central banks (like the Fed or ECB) use **interest rates** to manage the economy. Interest rates are arguably the single most important price in all of finance.
    * **Business Cycles:** Learn why economies expand and contract. This informs sector rotation and long-term asset allocation.

---

### Phase 2: The Quantitative Toolkit (Econometrics)

This is where economics, statistics, and your ML hobby merge. Econometrics is the use of statistical methods to analyze economic data.

* **Classical Linear Regression Model (CLRM):** You must know this inside and out. Understand its assumptions, interpretation of coefficients, $R^2$, and $p$-values.
* **Hypothesis Testing:** Move beyond ML prediction. In finance, *inference* (knowing *if* a variable has a statistically significant impact) is critical.
* **Time Series Analysis:** This is essential. Financial data is time-series data.
    * **Stationarity:** Why non-stationary data (like a stock price) breaks standard regression and how to fix it (e.g., using returns).
    * **Basic Models (AR, MA, ARMA):** Model a variable based on its own past.
    * **Volatility Modeling (ARCH/GARCH):** Critical for risk management and options pricing. You will model volatility itself as a time-varying process.

---

### Phase 3: Financial Economics (The Bridge to Markets)

This phase applies economic theory directly to financial assets.

* **Efficient Market Hypothesis (EMH):** The foundational theory you will try to disprove. Understand its three forms (weak, semi-strong, strong).
* **Portfolio Theory (Markowitz):** The mathematical basis for diversification. Understand the "efficient frontier" and the Sharpe ratio.
* **Asset Pricing Models:**
    * **CAPM (Capital Asset Pricing Model):** The classic model linking an asset's risk (Beta) to its expected return.
    * **Multi-Factor Models (Fama-French):** An evolution of CAPM. Argues that other "factors" (like company size or value) also drive returns. This is a core concept in modern quant strategy.

---

### Phase 4: Market Mechanics and Instruments

You must understand the specific products you will be modeling.

* **Equities (Stocks):**
    * **Valuation:** How to value a company (DCF, P/E ratios).
    * **Market Microstructure:** How does an exchange actually work? Understand order books, bid-ask spreads, and liquidity. This is key for execution algorithms.
* **Fixed Income (Bonds):**
    * **The Yield Curve:** Understand its shape and what it signals about future growth and inflation.
    * **Duration and Convexity:** These are the primary measures of a bond's sensitivity to interest rate changes. They are first and second derivatives.
* **Derivatives (The Quant Focus):**
    * **Options (Calls/Puts):** Understand their payoff structures.
    * **The Black-Scholes-Merton Model:** This is a core part of the quant canon. It connects to your physics interest (it's based on stochastic calculus related to heat diffusion).
    * **The "Greeks":** Delta, Gamma, Vega, Theta. These are partial derivatives of the option pricing model. They are used for hedging and risk management every day.