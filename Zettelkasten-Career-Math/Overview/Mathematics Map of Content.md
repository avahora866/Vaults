---
tags:
  - MOC
---
This is the top-level map connecting the foundational prerequisites to the core topics relevant to finance and quantum computing.

* [[Prerequisites MOC]]
* [[Core Topics MOC]]
***

### Probability and Statistics MOC
**Prerequisites:** [[Arithmetic MOC]], [[Algebra MOC]]
Probability is the mathematical study of chance and likelihood, quantifying the possibility of random events occurring. Statistics is the discipline that concerns the collection, organization, analysis, interpretation, and presentation of data. Together, statistics uses the principles of probability to analyse data, make inferences, and test hypotheses about populations based on samples.

* **Probability Distributions**
    * [[Normal Distribution]]: The normal distribution, often called the bell curve, is a continuous probability distribution symmetric around its mean, with data near the mean being more frequent. Many natural phenomena follow this distribution, making it fundamental in statistics for hypothesis testing and confidence intervals.
    * [[Binomial Distribution]]: The binomial distribution is a discrete probability distribution that models the number of successes in a fixed number of independent trials, each with the same probability of success. It's used when outcomes are binary (e.g., success/failure) and helps calculate the likelihood of a specific number of successes occurring.
    * [[Poisson Distribution]]: The Poisson distribution is a discrete probability distribution expressing the probability of a given number of events occurring in a fixed interval of time or space. It's applicable when events happen independently at a constant average rate, like the number of calls arriving at a call center per hour.
* **Statistical Methods**
    * [[Statistical Inference]]: Statistical inference is the process of drawing conclusions about a larger population based on data collected from a smaller sample. It uses probability theory to make estimates, predictions, or decisions while accounting for randomness and uncertainty.
    * [[Hypothesis Testing]]: Hypothesis testing is a formal statistical procedure used to determine whether there is enough evidence in sample data to infer that a certain condition is true for the entire population. It involves setting up a null hypothesis (no effect or difference) and an alternative hypothesis, then using data to decide which is more likely.
    * [[Confidence Intervals]]: A confidence interval provides a range of plausible values for an unknown population parameter, based on sample data. It quantifies the uncertainty associated with an estimate, indicating how confident we are that the true population value lies within the calculated interval.
    * [[Bayesian Methods]]: Bayesian methods are a statistical approach where probabilities are updated as more evidence becomes available, using Bayes' theorem. This approach incorporates prior knowledge or beliefs about parameters and combines them with observed data to arrive at an updated (posterior) probability distribution.
* **Computational Techniques**
    * [[Monte Carlo Simulations]]: Monte Carlo simulations are computational algorithms that rely on repeated random sampling to obtain numerical results, often used when analytical solutions are difficult or impossible. They model the probability of different outcomes in processes with inherent uncertainty by running numerous trial runs (simulations).
* **Applications**
    * [[Financial Modeling and Analysis]]: Financial modeling involves creating mathematical representations of a company's financial performance to forecast future outcomes and support decision-making. Financial analysis then uses these models and other data to evaluate investments, assess risk, and understand a company's overall financial health.

***

### Calculus MOC
**Prerequisites:** [[Algebra MOC]], [[Arithmetic MOC]]
Calculus is the mathematical study of continuous change. It is primarily divided into two branches: Differential Calculus, which focuses on instantaneous rates of change and the slopes of curves, and Integral Calculus, which concerns the accumulation of quantities and the areas under or between curves. The Fundamental Theorem of Calculus links these two concepts, establishing that differentiation and integration are inverse operations.

* **Differential Calculus**
    * [[Derivatives]]: Derivatives measure the instantaneous rate at which a function's output changes with respect to its input. They represent the slope of the tangent line to the function's graph at a specific point.
    * [[Rates of Change]]: Rates of change describe how quickly one quantity changes in relation to another quantity. Differential calculus provides the tools, primarily derivatives, to calculate these instantaneous rates precisely.
    * [[Motion Equations]]: Calculus is used to describe motion by relating position, velocity, and acceleration through derivatives and integrals. Velocity is the derivative of position, and acceleration is the derivative of velocity.
* **Integral Calculus**
    * [[Integrals]]: Integrals represent the accumulation of quantities and are the inverse operation of differentiation. They are often used to find the total amount of something when its rate of change is known, essentially summing up infinitely small parts.
    * [[Area Under Curves]]: Calculating the area under curves is a primary application of integration, representing the accumulation of the function's values over an interval. This concept extends to finding volumes, lengths, and other cumulative measures.
* **Advanced Topics**
    * [[Multivariable Calculus]]: Multivariable calculus extends the concepts of calculus to functions involving multiple independent variables. It deals with partial derivatives, multiple integrals, and vector calculus to analyze higher-dimensional spaces.
    * [[Optimization (Calculus)]] (e.g., [[Gradient Descent]]): Optimization in calculus involves using derivatives to find the maximum or minimum values (extrema) of functions, subject to certain constraints. Techniques like gradient descent iteratively move towards the minimum of a function, guided by its derivative.
* **Applications**
    * [[Derivative Pricing]]: In finance, calculus, particularly stochastic calculus, is crucial for derivative pricing, such as options and futures. Models like the Black-Scholes equation use concepts like rates of change and integration to determine the fair value of these complex financial instruments.

***

### Discrete Mathematics MOC
**Prerequisites:** [[Algebra MOC]]
Discrete mathematics is the study of mathematical structures that are fundamentally discrete, meaning they are countable or composed of distinct, separate parts, rather than being continuous. It covers topics such as logic, set theory, graph theory, and combinatorics. This field is essential for computer science, providing the foundation for algorithms, data structures, and cryptography.

* **Core Structures**
    * [[Graph Theory]]: Graph theory is the study of graphs, which are mathematical structures used to model relationships between objects (represented as vertices connected by edges). It analyzes properties like connectivity, paths, and cycles to solve problems in networks, logistics, and computer science.
    * [[Set Theory]]: Set theory is the foundational branch of mathematics dealing with collections of objects, called sets. It establishes the basic language and concepts for defining mathematical objects and reasoning about their properties and relationships.
    * [[Logic]]: Logic in discrete mathematics is the formal study of reasoning and inference, using symbolic rules to determine the validity of arguments. It provides the principles for constructing proofs, designing digital circuits, and ensuring correctness in software.
* **Applications**
    * [[Algorithms]]: Algorithms are step-by-step procedures or formulas for solving problems or performing computations, often operating on discrete structures like graphs or sets. The design and analysis of algorithm efficiency are central applications of discrete mathematics in computer science.
    * [[Computational Systems]]: Computational systems, including computer hardware and software, are fundamentally based on discrete mathematical principles. Concepts from logic, set theory, and graph theory are used to design circuits, databases, operating systems, and networks.

***

### Numerical Methods MOC
**Prerequisites:** [[Linear Algebra MOC]], [[Calculus MOC]]
Numerical methods are a set of mathematical techniques used to find approximate solutions to problems that are too complex to solve exactly, or analytically. These methods use iterative algorithms and computational power to solve problems in areas like differential equations, linear algebra, and optimization. Numerical analysis is the study of these algorithms, focusing on their accuracy, efficiency, and stability.

* **Core Techniques**
    * [[Root Finding]]: Root finding algorithms are numerical methods used to find the values (roots) where a function equals zero. These techniques, like the Newton-Raphson method, iteratively approximate solutions when analytical methods are impractical.
    * [[Numerical Optimization]]: Numerical optimization involves using iterative algorithms to find the input values that yield the maximum or minimum output of a function, often subject to constraints. It is essential when analytical optimization is infeasible due to the complexity of the objective function or constraints.
    * [[Matrix Decomposition]]: Matrix decomposition techniques factorize a matrix into a product of simpler matrices, revealing its properties and making computations easier. Methods like LU decomposition or QR factorization are fundamental for solving linear systems, eigenvalue problems, and data analysis.
* **Applications**
    * [[Financial Simulations]]: Financial simulations, often employing techniques like Monte Carlo methods, model the behavior of financial systems or instruments under uncertainty. They help quantify risk, price complex derivatives, and evaluate investment strategies by generating numerous possible future scenarios.

***

### Optimization MOC
**Prerequisites:** [[Calculus MOC]], [[Linear Algebra MOC]]
Optimization is the mathematical field focused on selecting the best element from a set of available alternatives by systematically choosing input values. It involves defining an objective function, which is the quantity you want to maximize (like profit) or minimize (like cost), and a set of constraints that limit the possible solutions. This methodology is widely applied in engineering, finance, operations research, and machine learning to find the most efficient or effective solution to a problem.

* **Key Problems**
    * [[Portfolio Optimization]]: Portfolio optimization is the process of selecting the best allocation of assets within an investment portfolio to maximize expected return for a given level of risk, or minimize risk for a target return. It uses mathematical models to balance the trade-offs between different assets based on their statistical properties like correlation and volatility.
    * [[Resource Allocation]]: Resource allocation involves distributing available resources, such as capital, labor, or materials, among competing activities or projects to achieve a desired outcome, like maximizing profit or minimizing cost. Optimization techniques are used to find the most efficient distribution under given constraints, such as budget limits or capacity restrictions.
* **Concepts**
    * [[Constrained Optimization]]: Constrained optimization is a branch of optimization that deals with finding the optimal solution to a problem where the variables must satisfy certain limitations or constraints. The challenge lies in finding the maximum or minimum of an objective function within the feasible region defined by these constraints.

***

### Time Series Analysis MOC
**Prerequisites:** [[Probability and Statistics MOC]]
Time series analysis is a statistical technique used to analyze sequences of data points collected at consistent intervals over time. The main goal is to identify and model the underlying components of the data, such as trends, seasonal variations, and cyclical patterns. By understanding this historical behavior, analysts can use these models to forecast future values, which is essential in fields like finance, economics, and meteorology.

* **Core Concepts**
    * [[Analysis of Sequential Data]]: Analysis of sequential data involves examining data points ordered over time or another sequence to identify patterns, dependencies, and structures. Techniques used aim to understand how past observations influence future ones, which is fundamental in time series analysis and forecasting.
* **Applications**
    * [[Stock Market Forecasting]]: Stock market forecasting is the attempt to predict the future value of company stock prices or overall market movements. Analysts use historical data, technical indicators, and fundamental analysis to make these predictions, although the inherent volatility of markets makes accurate forecasting challenging.
    * [[Financial Forecasting]]: Financial forecasting is the process of estimating a company's future financial performance, including revenues, expenses, and cash flow, based on historical data and assumptions about future conditions. It is a crucial tool for budgeting, planning investments, and guiding strategic business decisions.

***

### Stochastic Processes MOC
**Prerequisites:** [[Probability and Statistics MOC]], [[Calculus MOC]]
A stochastic process is a mathematical object defined as a collection of random variables, which is often used to model systems that evolve randomly over time. Unlike a deterministic process which follows a predictable path, a stochastic process is governed by chance, and its future state is described in terms of probabilities. This field is essential for modeling phenomena with inherent uncertainty, such as the movement of stock prices (random walks), the decay of a radioactive atom, or the flow of customers in a queue (Markov chains).

* **Core Concepts**
    * [[Random Processes]]: Random processes, also known as stochastic processes, are collections of random variables used to model systems that evolve unpredictably over time or space. They provide a mathematical framework for describing phenomena governed by chance, where future states are probabilistic rather than deterministic.
    * [[Systems with Inherent Randomness]]: Systems with inherent randomness are those whose future behavior cannot be precisely predicted due to underlying chance elements or unpredictable fluctuations. Stochastic processes are essential for modeling and analyzing these systems, capturing the uncertainty and statistical properties of their evolution.
* **Applications**
    * [[Financial Derivative Pricing]]: Financial derivative pricing involves determining the fair value of financial contracts, like options or futures, whose value depends on the price of an underlying asset. Stochastic processes, particularly models like Geometric Brownian Motion, are fundamental tools used to simulate the random price movements of these underlying assets, enabling the calculation of derivative prices.

***

### Game Theory MOC
**Prerequisites:** [[Algebra MOC]], [[Probability and Statistics MOC]]
Game theory is the mathematical study of strategic interactions, analyzing how rational decision-makers behave when their outcomes depend on the choices of others. The core objective is to determine the optimal strategy for a "player" by considering the potential actions and reactions of all other participants in a given "game." Key concepts like the Nash Equilibrium, where no player can benefit by unilaterally changing their strategy, are used to model competition and cooperation in economics, politics, biology, and computer science.

* **Core Concepts**
    * [[Strategic Interactions]]: Strategic interactions occur when the outcome of an individual's choice depends not only on their own action but also on the actions taken by other rational decision-makers. Game theory models these situations, analyzing how players anticipate and respond to each other's decisions to achieve their own objectives.
    * [[Mathematical Decision-Making]]: Mathematical decision-making involves using formal models and logical reasoning, often grounded in utility theory and probability, to analyze choices and select the optimal course of action. In game theory, this framework helps predict how rational players will behave in strategic situations by calculating expected payoffs for different strategies.

***

### Financial Mathematics MOC
**Prerequisites:** [[Calculus MOC]], [[Probability and Statistics MOC]], [[Linear Algebra MOC]]
Financial mathematics is a field of applied mathematics concerned with modeling financial markets and solving financial problems. It uses tools from probability, statistics, stochastic processes, and economic theory to value financial instruments, such as derivatives, and to manage investment portfolios. The primary goals are to quantify and manage risk, price assets accurately, and optimize investment strategies.

* **Fundamental Concepts**
    * [[Time Value of Money]]: The time value of money (TVM) is the concept that a sum of money is worth more now than the same sum will be at a future date due to its potential earning capacity. This core principle holds that money can earn interest, so any amount of money is worth more the sooner it is received.
* **Derivatives and Pricing**
    * [[Options Pricing]]: Options pricing models estimate the theoretical value of an options contract, which gives the holder the right, but not the obligation, to buy or sell an underlying asset at a specified price before a certain date. These models consider factors like the underlying asset's price, strike price, time to expiration, volatility, and interest rates to determine a fair premium.
    * [[Black-Scholes Model]]: The Black-Scholes model is a mathematical equation used to determine the theoretical price of European-style options, assuming factors like constant volatility and interest rates. It provides a formula that relates the option price to the current stock price, strike price, time to expiration, risk-free rate, and volatility.
    * [[The Greeks (Finance)]]: The Greeks are a set of risk measures, denoted by Greek letters (like Delta, Gamma, Vega, Theta, Rho), that quantify the sensitivity of a derivative's price to changes in underlying parameters. For example, Delta measures the rate of change of the option price with respect to changes in the underlying asset's price.
* **Risk Management**
    * [[Value at Risk (VaR)]]: Value at Risk (VaR) is a statistical measure used to quantify the level of financial risk within a firm or investment portfolio over a specific time frame. It estimates the maximum potential loss expected (e.g., in monetary value) over a given period under normal market conditions at a certain confidence level.
* **Forecasting**
    * [[Time Series Analysis]]: Time series analysis is a statistical technique used to analyze sequences of data points collected at consistent intervals over time. The primary goal is to identify underlying patterns like trends and seasonality, often to forecast future values based on historical behavior.