---
tags:
  - MachineLearning
  - "#SECONDARY_BRANCH"
---
Linear Regression is the simplist type of regression. The core idea is that you assume the output is a linear combination of the input variables.

Mathematically, for a single feature $x$
$$
y=w⋅x+b
$$

Where:
- y is the predicted value (dependent variable)
- x is the input feature (independent variable)
- w is the weight (how much x affects y)
- b is the bias or intercept (the value of y when 𝑥 = 0)

If you have multiple features 𝑥1,𝑥2,...,𝑥𝑛, it generalizes to:

$$
y=w1​x1​+w2​x2​+...+wn​xn​+b
$$

The goal of linear regression is to find the weights 𝑤 and bias 𝑏 that make the predictions 𝑦 as close as possible to the actual values in your dataset. Usually, we measure this closeness using a loss function, typically the mean squared error (MSE):
$$
MSE=N1​i=1∑N​(yi​−y^​i​)2
$$

Where 𝑦𝑖 is the actual value and 𝑦^𝑖 is the predicted value. The model tries to minimize this error