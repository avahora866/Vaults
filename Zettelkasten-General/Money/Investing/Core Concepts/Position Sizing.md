---
tags:
  - Money
  - Investing
  - LEAF
---
**Position sizing** refers to the process of determining how much capital (or how many shares/contracts) you should allocate to a particular trade. It's a key component of **risk management** in trading and investing.

# Why Position Sizing Matters

It helps you:

* **Control risk** per trade.
* **Avoid overexposure** to a single asset.
* **Ensure consistency** in your trading approach.
* **Prevent large drawdowns** that could wipe out your account.

---

# How Position Sizing Works

At its core, you calculate position size based on:

* Your **account size**
* The **amount of risk** you are willing to take per trade (often a percentage of your total account)
* The **distance between your entry price and stop-loss**

## Basic Formula:

Position Size=Account RiskTrade Risk per Unit\\text{Position Size} \= \\frac{\\text{Account Risk}}{\\text{Trade Risk per Unit}}

Where:

* **Account Risk** \= Account size × % risk per trade (e.g., 1% of $10,000 \= $100)
* **Trade Risk per Unit** \= Entry price – Stop-loss price (e.g., $50 – $48 \= $2)

**Example:**

* Account size: $10,000
* Risk per trade: 1% \= $100
* Entry price: $50
* Stop-loss: $48
* Trade risk per share: $2

Position Size=1002=50 shares\\text{Position Size} \= \\frac{100}{2} \= 50 \\text{ shares}

You’d buy **50 shares** so that, if the trade hits your stop-loss, you lose **exactly $100** (1% of your account).

---

# Common Position Sizing Methods

1. **Fixed Dollar Amount**: You invest the same dollar amount in each trade.
2. **Fixed Risk Percentage**: You risk the same percentage of your account on each trade (more common in professional strategies).
3. **Volatility-Based**: Adjust your position size based on the volatility of the asset (more volatile \= smaller position).
4. **Kelly Criterion**: A formula-based approach to optimize position size for maximum growth (complex and not always practical).

---

# In a Trading Bot

You can program your bot to:

* Dynamically calculate position size based on your risk parameters
* Enforce max limits (e.g., never risk more than 2% of capital)
* Adjust based on account growth or shrinkage over time

---


