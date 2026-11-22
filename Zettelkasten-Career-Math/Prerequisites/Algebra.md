---
tags:
  - Algebra
  - TRUNK
---
# 1. Core Concepts

- **Variable:** A symbol (usually a letter) representing an unknown value.
- **Constant:** A fixed numerical value.
- **Coefficient:** The number multiplying a variable. In $5x^2$, the coefficient is 5. A lone variable like $x$ has an implied coefficient of 1.
- **Expression:** A combination of variables, constants, and operators — e.g. $3x + 5$. Does not assert equality.
- **Equation:** Two expressions set equal — e.g. $2x + 3 = 7$. Has a solution to find.

---

# 2. Operations in Algebra

| Operation | With variables | Example |
|-----------|---------------|---------|
| Addition | Combine like terms (same variable & power) | $3x + 5x = 8x$ |
| Subtraction | Subtract coefficients of like terms | $9y - 4y = 5y$ |
| Multiplication | Multiply coefficients; add exponents of same base | $x^2 \cdot x^3 = x^5$ |
| Division | Divide coefficients; subtract exponents | $x^5 \div x^2 = x^3$ |

---

# 3. Equations

An equation is true for specific value(s) of the variable. Goal: isolate the variable using inverse operations.

## Solving Strategy

| Equation Type | Approach |
|---------------|----------|
| One-step | Single inverse operation |
| Two-step | Undo add/sub first, then mult/div |
| Variables on both sides | Move all variables to one side |
| With fractions | Multiply through by LCD, or cross-multiply |

## Examples

**Two-step:** $3x + 2 = 11$ → $3x = 9$ → $x = 3$

**Both sides:** $4x - 2 = 2x + 6$ → $2x = 8$ → $x = 4$

**Fractions (LCD):** $\frac{1}{3}x + \frac{1}{2} = \frac{5}{6}$, multiply by 6: $2x + 3 = 5$ → $x = 1$

## Special Cases

| Result | Situation |
|--------|-----------|
| Unique solution | One value satisfies the equation |
| Infinite solutions | Simplifies to a true statement e.g. $5 = 5$ |
| No solution | Simplifies to a contradiction e.g. $-2 = 20$ |

---

# 4. Cross-Multiplying

Used to solve proportions (two fractions set equal):

$$\frac{a}{b} = \frac{c}{d} \implies ad = bc$$

**Example:**
$$\frac{x+2}{x-1} = \frac{3}{4} \implies 4(x+2) = 3(x-1) \implies 4x + 8 = 3x - 3 \implies x = -11$$

> Only valid when **two fractions are equal to each other** — don't use for addition/subtraction of fractions. Always check for values that make a denominator zero.

---

# 5. Linear Equations

Highest power of the variable is **1**.

$$ax + b = 0 \qquad \text{(one variable)}$$
$$ax + by = c \qquad \text{(two variables — graphs as a straight line)}$$

## Forms

| Form | Expression | Notes |
|------|------------|-------|
| Standard | $ax + by = c$ | General form |
| Slope-intercept | $y = mx + c$ | $m$ = slope, $c$ = y-intercept |
| Point-slope | $y - y_1 = m(x - x_1)$ | For line through $(x_1, y_1)$ |

## Slope
$$m = \frac{y_2 - y_1}{x_2 - x_1}$$

## Systems of Linear Equations

Solve two equations simultaneously — methods: **substitution**, **elimination**, or **graphing**.

**Example (substitution):**
$$\begin{cases} x + y = 5 \\ 2x - y = 3 \end{cases}$$
Substitute $y = 5 - x$: $2x - (5 - x) = 3 \Rightarrow x = \frac{8}{3},\quad y = \frac{7}{3}$

---

# 6. Inequalities

Like equations but use $<,\ \leq,\ >,\ \geq,\ \neq$ instead of $=$.

**Key rule:** multiplying or dividing by a **negative number reverses the inequality sign**.

$$-2x \geq 6 \implies x \leq -3$$

## Graphing on a Number Line
- **Open circle:** strict inequality ($<$ or $>$)
- **Closed circle:** inclusive ($\leq$ or $\geq$)
- Shade left for $<$/$\leq$, right for $>$/$\geq$

## Compound Inequalities

| Type | Meaning | Example |
|------|---------|---------|
| **And** | Both conditions true (intersection) | $-1 < x \leq 2$ |
| **Or** | At least one true (union) | $x < 1$ or $x \geq 3$ |

**And example:** $1 < 2x + 3 \leq 7 \implies -1 < x \leq 2$

---

# 7. Functions

A function assigns **exactly one output** to each input.

$$f(x) = 2x + 3 \quad \Rightarrow \quad f(4) = 11$$

- **Domain:** all valid input values
- **Range:** all possible output values
- **Vertical line test:** if a vertical line hits the graph more than once → not a function

## Common Types

| Type | Form | Graph |
|------|------|-------|
| Linear | $f(x) = mx + b$ | Straight line |
| Quadratic | $f(x) = ax^2 + bx + c$ | Parabola |
| Polynomial | $f(x) = a_nx^n + \cdots$ | Curve |
| Rational | $f(x) = \frac{p(x)}{q(x)}$ | May have asymptotes |
| Exponential | $f(x) = a^x$ | Growth/decay curve |
| Logarithmic | $f(x) = \log_a(x)$ | Inverse of exponential |

## Operations on Functions

$$(f + g)(x) = f(x) + g(x) \qquad (fg)(x) = f(x) \cdot g(x) \qquad \left(\frac{f}{g}\right)(x) = \frac{f(x)}{g(x)},\ g(x) \neq 0$$

## Composition
$$(f \circ g)(x) = f(g(x))$$
Order matters: $f(g(x)) \neq g(f(x))$ in general.

## Inverse Functions
Reverses the original function. To find $f^{-1}(x)$: write $y = f(x)$, swap $x$ and $y$, solve for $y$.

**Example:** $f(x) = 2x + 3 \implies f^{-1}(x) = \frac{x - 3}{2}$

---

# 8. Graphing

Algebra gives symbolic rules; graphs make them visual. Every equation can be plotted on the Cartesian plane ($x$-axis horizontal, $y$-axis vertical).

## Equation Types and Their Graphs

| Equation | Graph Shape |
|----------|-------------|
| $y = mx + b$ (linear) | Straight line |
| $y = ax^2 + bx + c$ (quadratic) | Parabola; $a > 0$ opens up, $a < 0$ opens down |
| $y = x^3 - 4x$ (cubic) | S-shaped curve |
| $y = \frac{p(x)}{q(x)}$ (rational) | Curves with asymptotes |
| $y = a^x$ / $y = \ln(x)$ | Exponential growth/decay |
| $y = \sin(x)$, $\cos(x)$ | Repeating wave |

## Transformations

| Algebraic change | Visual effect |
|-----------------|---------------|
| $f(x) + k$ | Shift up/down |
| $f(x - h)$ | Shift left/right |
| $af(x)$ | Stretch/compress vertically |
| $-f(x)$ | Reflect across x-axis |

## Key Graph Features
- **Intercepts:** where graph crosses axes (solutions)
- **Slope:** rate of change
- **Inequalities:** shade a region rather than draw a line

---

# 9. Polynomials

$$P(x) = a_nx^n + a_{n-1}x^{n-1} + \cdots + a_1x + a_0 \qquad (a_n \neq 0)$$

- **Degree:** highest power with nonzero coefficient
- **Leading coefficient:** $a_n$

## Terminology

| Name | Terms | Example |
|------|-------|---------|
| Monomial | 1 | $3x^2$ |
| Binomial | 2 | $x + 2$ |
| Trinomial | 3 | $x^2 + 3x + 1$ |

## Operations

**Add/subtract:** combine like terms
$$(2x^2 + 3x + 1) + (x^2 - x - 4) = 3x^2 + 2x - 3$$

**Multiply:** distribute each term
$$(x + 2)(x - 3) = x^2 - x - 6$$

**Divide:** long division or synthetic division (for linear divisors)

## Key Theorems
- **Fundamental Theorem of Algebra:** degree $n$ polynomial has exactly $n$ complex roots (including multiplicities)
- **Factor Theorem:** if $P(a) = 0$, then $(x - a)$ is a factor
- **Rational Root Theorem:** possible rational roots are $\pm\frac{p}{q}$ where $p$ | constant term, $q$ | leading coefficient

## End Behaviour
- **Even degree, positive leading coeff:** both ends up
- **Even degree, negative leading coeff:** both ends down
- **Odd degree:** ends go in opposite directions

## Polynomial Identities
$$(a + b)^2 = a^2 + 2ab + b^2 \qquad (a - b)^2 = a^2 - 2ab + b^2 \qquad a^2 - b^2 = (a-b)(a+b)$$

---

# 10. Factoring

Rewriting an expression as a product of simpler expressions.

## Strategy
1. Factor out the **GCF** first
2. Count terms: 2 → difference of squares/cubes; 3 → trinomial; 4 → grouping
3. Check for special patterns
4. Verify by expanding back

## Special Patterns

| Pattern | Formula | Example |
|---------|---------|---------|
| Difference of squares | $a^2 - b^2 = (a-b)(a+b)$ | $x^2 - 9 = (x-3)(x+3)$ |
| Perfect square (+) | $a^2 + 2ab + b^2 = (a+b)^2$ | $x^2 + 6x + 9 = (x+3)^2$ |
| Perfect square (−) | $a^2 - 2ab + b^2 = (a-b)^2$ | $x^2 - 10x + 25 = (x-5)^2$ |
| Sum of cubes | $a^3 + b^3 = (a+b)(a^2 - ab + b^2)$ | $x^3 + 8 = (x+2)(x^2 - 2x + 4)$ |
| Difference of cubes | $a^3 - b^3 = (a-b)(a^2 + ab + b^2)$ | $x^3 - 27 = (x-3)(x^2 + 3x + 9)$ |

## Factoring by Grouping (4 terms)
$$x^3 + 2x^2 + 3x + 6 = x^2(x+2) + 3(x+2) = (x^2+3)(x+2)$$
