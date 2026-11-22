---
tags:
  - SECONDARY_BRANCH
---

# Scalar
A scalar is a **single number**. It only has **magnitude (size)**. A vector is a list of numbers. It has both magnitude and direction.

Think of a scalar as a simple measurement:
- $5$ (just the number)
- $10 \text{ km/h}$ (a speed)
- $-2.5$
- $30^\circ \text{C}$ (a temperature)

These values tell you "how much" of something there is, but not "which way" it is pointing. The name scalar comes from the fact that it scales things. If you multiply something by the scalar $5$, you make it $5$ times bigger.

In linear algebra, we use scalars (like $c$) to operate on vectors (like $\mathbf{v}$). For example, in scalar multiplication, we scale the vector.

# Operations
## Addition
To add two vectors, you add their corresponding components.

Geometrically: 
Think of it as "tip-to-tail". If you have vector $\mathbf{u}$ and vector $\mathbf{v}$, you place the tail (start) of $\mathbf{v}$ at the tip (head) of $\mathbf{u}$. The new vector, $\mathbf{u} + \mathbf{v}$, is the arrow that goes from the tail of $\mathbf{u}$ to the tip of $\mathbf{v}$.

Algebraically:
Let $\mathbf{u} = \begin{bmatrix} 3 \\ 1 \end{bmatrix}$ and $\mathbf{v} = \begin{bmatrix} 2 \\ 4 \end{bmatrix}$.
$$\mathbf{u} + \mathbf{v} = \begin{bmatrix} 3 + 2 \\ 1 + 4 \end{bmatrix} = \begin{bmatrix} 5 \\ 5 \end{bmatrix}$$

## Subtraction
To subtract two vectors, you subtract their corresponding components.
Geometrically: 
$\mathbf{u} - \mathbf{v}$ is the same as adding a negative vector: $\mathbf{u} + (-\mathbf{v})$. The vector $-\mathbf{v}$ has the same length as $\mathbf{v}$ but points in the exact opposite direction.

Algebraically:
Using the same vectors:
$$\mathbf{u} - \mathbf{v} = \begin{bmatrix} 3 - 2 \\ 1 - 4 \end{bmatrix} = \begin{bmatrix} 1 \\ -3 \end{bmatrix}$$

## Scalar Multiplication
Scalar multiplication involves multiplying a scalar (a single number) by a vector.
You simply multiply every component of the vector by that scalar. This operation scales the vector.

Algebraically:
Let $c$ be a scalar and $\mathbf{v} = \begin{bmatrix} x \\ y \end{bmatrix}$.
$$c\mathbf{v} = c \cdot \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} c \cdot x \\ c \cdot y \end{bmatrix}$$

Example: Let $c = 3$ and $\mathbf{v} = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$.
$$3\mathbf{v} = 3 \cdot \begin{bmatrix} 1 \\ 2 \end{bmatrix} = \begin{bmatrix} 3 \cdot 1 \\ 3 \cdot 2 \end{bmatrix} = \begin{bmatrix} 3 \\ 6 \end{bmatrix}$$

Geometric Effect
Multiplying a vector by a scalar changes its magnitude (length) and can reverse its direction.
c = scalar
- If $c > 1$: The vector is stretched. (e.g., $c = 3$ makes it 3 times longer).
- If $0 < c < 1$: The vector is shrunk. (e.g., $c = 0.5$ makes it half as long).
- If $c = -1$: The vector's direction is reversed.
- If $c < -1$: The vector is reversed and stretched.
- If $-1 < c < 0$: The vector is reversed and shrunk.

## Dot Product
The dot product (or scalar product) is an operation that takes two vectors and returns a single scalar number. It's a way of "multiplying" two vectors to get a number that tells you something about their relationship.

Algebraic Formula:
You calculate it by multiplying corresponding components and adding the results. 
For two 2D vectors $\mathbf{a} = \begin{bmatrix} a_1 \\ a_2 \end{bmatrix}$ and $\mathbf{b} = \begin{bmatrix} b_1 \\ b_2 \end{bmatrix}$:$\mathbf{a} \cdot \mathbf{b} = a_1b_1 + a_2b_2$

Example:
$\mathbf{a} = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$ and $\mathbf{b} = \begin{bmatrix} 3 \\ 4 \end{bmatrix}$.$\mathbf{a} \cdot \mathbf{b} = (1 \cdot 3) + (2 \cdot 4) = 3 + 8 = 11$

What it means: The dot product measures how much one vector "goes in the direction" of the other.
- If $\mathbf{a} \cdot \mathbf{b}$ is positive, the vectors point in generally the same direction (angle $< 90^\circ$).
- If $\mathbf{a} \cdot \mathbf{b}$ is negative, they point in generally opposite directions (angle $> 90^\circ$).
- If $\mathbf{a} \cdot \mathbf{b} = 0$, the vectors are orthogonal (perpendicular, $90^\circ$). This is the most important use of the dot product.

## Cross Product (3D only)
The cross product is an operation on two 3D vectors that produces a new vector. This operation is only defined in three dimensions.
Resulting Vector: Let $\mathbf{c} = \mathbf{a} \times \mathbf{b}$. The resulting vector $\mathbf{c}$ has two special properties:
- Direction: $\mathbf{c}$ is orthogonal (perpendicular) to both $\mathbf{a}$ and $\mathbf{b}$. You can find its direction using the "right-hand rule".
- Magnitude: The length of $\mathbf{c}$ ($\|\mathbf{c}\|$) is equal to the area of the parallelogram formed by vectors $\mathbf{a}$ and $\mathbf{b}$.

Algebraic Formula:
$\mathbf{a} = \begin{bmatrix} a_1 \\ a_2 \\ a_3 \end{bmatrix}$ and $\mathbf{b} = \begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix}$
$$\mathbf{a} \times \mathbf{b} = \begin{bmatrix} a_2b_3 - a_3b_2 \\ a_3b_1 - a_1b_3 \\ a_1b_2 - a_2b_1 \end{bmatrix}$$

Example:
$\mathbf{a} = \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}$ (x-axis) and $\mathbf{b} = \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}$ (y-axis)
$$\mathbf{a} \times \mathbf{b} = \begin{bmatrix} (0)(0) - (0)(1) \\ (0)(0) - (1)(0) \\ (1)(1) - (0)(0) \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}$$ (z-axis)
This makes sense: the z-axis is perpendicular to both the x-axis and y-axis.

# Linear Independence
This concept describes the relationship between vectors in a set. A set of vectors is **linearly independent** if no vector in the set can be written as a "linear combination" of the others. A linear combination is just adding vectors that have been scaled (scalar multiplication).

In simple terms, **linearly independent vectors** all point in "genuinely different" directions. No vector is redundant.

Formal Definition:
A set of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \dots, \mathbf{v}_k\}$ is linearly independent if the only way to make the following equation true is if all the scalars $c_1, c_2, \dots, c_k$ are zero.
$c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \dots + c_k\mathbf{v}_k = \mathbf{0}$ (the zero vector)

Example (Dependent):
$\mathbf{u} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$, $\mathbf{v} = \begin{bmatrix} 2 \\ 2 \end{bmatrix}$.
This set is linearly dependent. Why? Because $\mathbf{v}$ is not independent; it's just $2 \cdot \mathbf{u}$. We can write a non-zero equation: $2\mathbf{u} - 1\mathbf{v} = \mathbf{0}$. Since the scalars ($2$ and $-1$) are not zero, the set is dependent.

Example (Independent):
$\mathbf{u} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$, $\mathbf{v} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}$.
This set is linearly independent. There is no scalar $c$ you can multiply $\mathbf{u}$ by to get $\mathbf{v}$. They point in fundamentally different directions. The only way to make $c_1\begin{bmatrix} 1 \\ 0 \end{bmatrix} + c_2\begin{bmatrix} 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}$ true is if $c_1=0$ and $c_2=0$.

