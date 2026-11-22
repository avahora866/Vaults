---
tags:
  - TRUNK
---
At its core, algebra is a branch of mathematics where we use symbols (like $x$, $y$, or $a$) to represent unknown numbers. We call these symbols variables. Algebra provides a set of rules and operations (like addition, multiplication, etc.) that let us manipulate these variables within equations to figure out their unknown values.

For example, in the equation $2x + 3 = 11$, algebra gives us the methods to determine that $x$ must be $4$.

Linear algebra is a specific, powerful field within algebra. It focuses on linear equations and their representations. **A linear equation is one where variables aren't multiplied by each other or raised to powers** (like $x^2$ or $\sqrt{y}$).

For example, $3x + 2y - z = 5$ is a linear equation. But $3x^2 + 2y = 5$ is not linear because of the $x^2$.

An equation is linear if it creates a straight line when you graph it.

This straight-line behavior happens because of a few strict rules for the variables (like $x$, $y$, $z$):
1. Power of 1: The variables only appear to the first power. You'll see $x$, but never $x^2$, $x^3$, or $\sqrt{x}$ (which is $x^{1/2}$).
2. No Multiplying Variables: Variables are not multiplied by each other. You won't see terms like $x \cdot y$ or $3xy$.
3. No Special Functions: Variables are not inside functions like 3 $\sin(x)$, 4 $\log(x)$, or 5 $e^x$
    - Linear Example: $y = 3x - 1$. If you plot this, you get a straight line.
    - Non-Linear Example: $y = x^2$. The 7 $x^2$ term (rule #1) makes it a curve (a parabola)

Linear algebra deals with solving systems of these linear equations (often many equations with many variables) and the key objects used to study them: vectors, matrices, and vector spaces. It's fundamentally the study of straight lines, planes, and their transformations in space.

# Vector
[[Vector]]
A vector is an object that has both a **magnitude (length)** and a **direction**. The most common way to visualize this is as an arrow starting from an origin.
For example, "go 4 steps east and 2 steps north" is a vector. It tells you how far to go and which way to go.
In algebra, we represent this vector as a list of numbers (its coordinates) in brackets. We can write it as a column vector:
$$\mathbf{v} = \begin{bmatrix} 4 \\ 2 \end{bmatrix}$$
or as a row vector:
$$\mathbf{v} = [4, 2]$$
The above vector $\mathbf{v}$ represents a single point in a 2-dimensional (2D) plane, located at 4 units on the x-axis and 2 units on the y-axis.

# Matrix
[[Matrix]]
A matrix is a rectangular grid, or array, of numbers arranged in rows and columns.

We write matrices inside square brackets. A matrix with $m$ rows and $n$ columns is called an "$m \times n$" (read "m by n") matrix.For example, here is a $2 \times 3$ matrix (2 rows, 3 columns):
$$A = \begin{bmatrix} 1 & 4 & -2 \\ 5 & 0 & 3 \end{bmatrix}$$

At a basic level, matrices are a way to organize data. You can think of them as a spreadsheet.

Their real power in linear algebra comes from two main uses:
- Representing Systems: They are an incredibly efficient way to write down and solve systems of linear equations.
- Representing Transformations: They can describe geometric operations, like rotating, scaling, or shearing a vector in space

For example, we can take the system of equations from earlier: $x + y = 10$ and $2x - y = 8$
We can represent this entire system with one augmented matrix:
$$\left[ \begin{array}{cc|c} 1 & 1 & 10 \\ 2 & -1 & 8 \end{array} \right]$$

The first and second column holds the **coefficients** for $x$ and $y$, and the part after the line holds the constants. This compact format is the key to solving large, complex systems.


# Linear Systems
[[Linear Systems]]
A system of linear equations is a collection of two or more linear equations involving the same set of variables.
- Example 2D System:
	- $2x + 3y = 7$
	- $x - y = 1$
- Example 3D System:
	- $x + 2y - z = 4$
	- $3x - y + 5z = 2$
	- $2x + 2y - 2z = 8$
The solution to the system is a set of values (one for each variable) that makes all equations in the system true simultaneously.

This leads to three possibilities:
1. One Unique Solution: The lines/planes intersect at a single point.
2. No Solution: The lines are parallel, or the planes do not share a common intersection point.
3. Infinitely Many Solutions: The equations describe the same line, or the planes intersect along a common line.

The entire goal of this topic is to find that solution (or solutions) algebraically. To do that, we use the augmented matrix. It is the most direct way to represent the problem.
For the 2D system: $2x + 3y = 7$$x - y = 1$
The augmented matrix is:$\left[ \begin{array}{cc|c} 2 & 3 & 7 \\ 1 & -1 & 1 \end{array} \right]$
This matrix contains all the information of the system in a compact, numerical format. We now "solve" this matrix, rather than the equations.
# Tutorials
- https://www.youtube.com/watch?v=E8YQPBHLUrY