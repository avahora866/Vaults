---
tags:
  - SECONDARY_BRANCH
---


# Operations
## Basic Addition and Subtraction
This works exactly like vector addition. To add or subtract two matrices, they must have the exact same dimensions (e.g., both $2 \times 3$). You simply add or subtract the corresponding elements.
Addition Example:
Let $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ and $B = \begin{bmatrix} 10 & 20 \\ 30 & 40 \end{bmatrix}$.
$$A + B = \begin{bmatrix} 1+10 & 2+20 \\ 3+30 & 4+40 \end{bmatrix} = \begin{bmatrix} 11 & 22 \\ 33 & 44 \end{bmatrix}$$

Subtraction Example:
$$B - A = \begin{bmatrix} 10-1 & 20-2 \\ 30-3 & 40-4 \end{bmatrix} = \begin{bmatrix} 9 & 18 \\ 27 & 36 \end{bmatrix}$$

## Scalar Multiplication
This also works exactly like vector scalar multiplication. You multiply every element in the matrix by a single scalar number.
Example:
Let $c = 3$ and $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$.
$$c \cdot A = 3 \cdot \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} = \begin{bmatrix} 3 \cdot 1 & 3 \cdot 2 \\ 3 \cdot 3 & 3 \cdot 4 \end{bmatrix} = \begin{bmatrix} 3 & 6 \\ 9 & 12 \end{bmatrix}$$

## Matrix Multiplication
Matrix multiplication is the most complex of the basic operations because it is not element-wise. It's a "row-by-column" operation.

The operation combines a row from the first matrix with a column from the second matrix using a dot product.

### The Rule of Dimensions
To multiply matrix $A$ by matrix $B$ (to find $A \times B$):The number of columns in $A$ must equal the number of rows in $B$.If $A$ is $m \times n$ (m rows, n columns)And $B$ is $n \times p$ (n rows, p columns)The inner dimensions ($n$) match.The resulting matrix, $C$, will have dimensions $m \times p$.

### The "Row-by-Column" Method
To find the element in row $i$ and column $j$ of the resulting matrix $C$, you take the dot product of row $i$ from $A$ and column $j$ from $B$.

Example: Let $A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$ ($2 \times 2$) and $B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}$ ($2 \times 2$).The result $C$ will be a $2 \times 2$ matrix: $C = \begin{bmatrix} C_{11} & C_{12} \\ C_{21} & C_{22} \end{bmatrix}$.

1. Find $C_{11}$ (Row 1 of A $\cdot$ Column 1 of B):
$$\begin{bmatrix} 1 & 2 \end{bmatrix} \cdot \begin{bmatrix} 5 \\ 7 \end{bmatrix} = (1 \cdot 5) + (2 \cdot 7) = 5 + 14 = \mathbf{19}$$
2. Find $C_{12}$ (Row 1 of A $\cdot$ Column 2 of B):
$$\begin{bmatrix} 1 & 2 \end{bmatrix} \cdot \begin{bmatrix} 6 \\ 8 \end{bmatrix} = (1 \cdot 6) + (2 \cdot 8) = 6 + 16 = \mathbf{22}$$
3. Find $C_{21}$ (Row 2 of A $\cdot$ Column 1 of B):
$$\begin{bmatrix} 3 & 4 \end{bmatrix} \cdot \begin{bmatrix} 5 \\ 7 \end{bmatrix} = (3 \cdot 5) + (4 \cdot 7) = 15 + 28 = \mathbf{43}$$
4. Find $C_{22}$ (Row 2 of A $\cdot$ Column 2 of B):
$$\begin{bmatrix} 3 & 4 \end{bmatrix} \cdot \begin{bmatrix} 6 \\ 8 \end{bmatrix} = (3 \cdot 6) + (4 \cdot 8) = 18 + 32 = \mathbf{50}$$

Result:
$$C = A \times B = \begin{bmatrix} 19 & 22 \\ 43 & 50 \end{bmatrix}$$

Important: Matrix multiplication is not commutative. In almost all cases, $A \times B \neq B \times A$.

## Determinent
The determinant is a single scalar number calculated from a square matrix. It is denoted as $\det(A)$ or $|A|$. Its most important use is to determine if a matrix is invertible:
- If $\det(A) \neq 0$, the matrix $A$ has an inverse.
- If $\det(A) = 0$, the matrix $A$ is singular and does not have an inverse.

The determinant also has geometric meanings. For a 2D matrix, its absolute value represents the area of the parallelogram formed by its column vectors. For 3D, it's the volume of the parallelepiped. 

Calculation for a $2 \times 2$ Matrix
This is the simplest case. For a matrix $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$, the formula is:
$$\det(A) = ad - bc$$

You multiply the diagonals and subtract.
Example 1 (Invertible): Let $A = \begin{bmatrix} 4 & 1 \\ 2 & 3 \end{bmatrix}$.$\det(A) = (4 \cdot 3) - (1 \cdot 2) = 12 - 2 = 10$
Since $\det(A) = 10$ (which is not $0$), this matrix has an inverse.
Example 2 (Singular): Let $B = \begin{bmatrix} 1 & 2 \\ 2 & 4 \end{bmatrix}$.$\det(B) = (1 \cdot 4) - (2 \cdot 2) = 4 - 4 = 0$.
Since $\det(B) = 0$, this matrix is singular (it has no inverse). 

The calculation for a $3 \times 3$ matrix (and larger) is more involved, using a method called cofactor expansion.

### Cofactor expansion
Calculating the determinant for a $3 \times 3$ matrix is more complex. The standard method is called Cofactor Expansion.

You will break the $3 \times 3$ determinant down into three $2 \times 2$ determinants.
1. Pick one row or column. The first row is the most common choice.
2. Use the "checkerboard" of signs. A $3 \times 3$ matrix has a sign pattern for this operation:
$$\begin{bmatrix} + & - & + \\ - & + & - \\ + & - & + \end{bmatrix}$$
3. Go element by element across your chosen row (e.g., Row 1: $a$, $b$, $c$). For each element:
    - Take the element's sign from the checkerboard.
    - Multiply by the element itself.
    - Multiply by the determinant of the $2 \times 2$ matrix (its "minor") that remains after you cross out that element's row and column.

Example
Let $A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{bmatrix}$. We will expand along Row 1.
$$\det(A) = \mathbf{+1} \cdot \left| \begin{vmatrix} 5 & 6 \\ 8 & 9 \end{vmatrix} \right| \quad \mathbf{-2} \cdot \left| \begin{vmatrix} 4 & 6 \\ 7 & 9 \end{vmatrix} \right| \quad \mathbf{+3} \cdot \left| \begin{vmatrix} 4 & 5 \\ 7 & 8 \end{vmatrix} \right|$$
Now, calculate each $2 \times 2$ determinant:
1. $+1 \cdot ((5 \cdot 9) - (6 \cdot 8))$$= +1 \cdot (45 - 48)$$= \mathbf{-3}$
2. $-2 \cdot ((4 \cdot 9) - (6 \cdot 7))$$= -2 \cdot (36 - 42)$$= -2 \cdot (-6)$$= \mathbf{+12}$
3. $+3 \cdot ((4 \cdot 8) - (5 \cdot 7))$$= +3 \cdot (32 - 35)$$= +3 \cdot (-3)$$= \mathbf{-9}$
Finally, add the results:
$$\det(A) = -3 + 12 - 9 = \mathbf{0}$$
Since the determinant is $0$, we know this matrix $A$ is singular (it has no inverse).

## Inverse 
The inverse of a square matrix $A$, denoted $A^{-1}$, is the matrix that, when multiplied by $A$, results in the identity matrix $I$.It's the matrix equivalent of reciprocals in regular numbers. Just as $5 \cdot (1/5) = 1$, a matrix $A$ multiplied by its inverse $A^{-1}$ gives the identity $I$.
$A \times A^{-1} = I$$A^{-1} \times A = I$
As we just discussed, a matrix only has an inverse if its determinant is not zero.

How to Find the Inverse (for a $2 \times 2$ Matrix)
For a $2 \times 2$ matrix, $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$, there is a direct formula:
$$A^{-1} = \frac{1}{\det(A)} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$
To get the inverse, you:
1. Swap the elements $a$ and $d$.
2. Negate the elements $b$ and $c$.
3. Multiply the entire new matrix by $\frac{1}{\det(A)}$ (the reciprocal of the determinant).

Example
Let's use the matrix from our determinant example: $A = \begin{bmatrix} 4 & 1 \\ 2 & 3 \end{bmatrix}$. We calculated its determinant: $\det(A) = 10$. 
Now, we apply the inverse formula:
1. Swap $a$ and $d$: $\begin{bmatrix} 3 & 1 \\ 2 & 4 \end{bmatrix}$
2. Negate $b$ and $c$: $\begin{bmatrix} 3 & -1 \\ -2 & 4 \end{bmatrix}$
3. Multiply by $\frac{1}{\det(A)}$:$A^{-1} = \frac{1}{10} \begin{bmatrix} 3 & -1 \\ -2 & 4 \end{bmatrix} = \begin{bmatrix} 3/10 & -1/10 \\ -2/10 & 4/10 \end{bmatrix}$

We can check this. If we multiply $A \times A^{-1}$, we should get the identity matrix $\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$.

(Finding the inverse for $3 \times 3$ and larger matrices is a much more complex process, often done using a method called Gauss-Jordan elimination, which is related to solving systems of equations.)

# Types of Matrix

## Square Matrix
A matrix is square if it has the same number of rows as columns. 
- A $2 \times 2$, $3 \times 3$, or $n \times n$ matrix is square. 
- A $2 \times 3$ matrix is not square.
This type is important because many operations, like finding determinants or inverses, are only defined for square matrices.
Example ($3 \times 3$):
$$\begin{bmatrix} 4 & 0 & -1 \\ 2 & 1 & 7 \\ 9 & -5 & 3 \end{bmatrix}$$

## Diagonal Matrix
A diagonal matrix is a square matrix where all entries off the main diagonal are zero. The main diagonal (top-left to bottom-right) can have any numbers.
Example:
$$\begin{bmatrix} 3 & 0 & 0 \\ 0 & -1 & 0 \\ 0 & 0 & 7 \end{bmatrix}$$

## Identity Matrix ($I$)
The identity matrix, written as $I$, is a special type of diagonal matrix where every element on the main diagonal is 1. The identity matrix is the matrix equivalent of the number 1. Multiplying any matrix $A$ by the identity matrix $I$ just gives you $A$ back ( $A \times I = A$ ).
Example ($3 \times 3$ Identity):
$$I = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

## Transpose ($A^T$)
The transpose of a matrix, written $A^T$, is a new matrix formed by swapping its rows and columns. The first row of $A$ becomes the first column of $A^T$. The second row of $A$ becomes the second column of $A^T$. If $A$ has dimensions $m \times n$, then $A^T$ will have dimensions $n \times m$.
Example:
$$A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}$$
(This is a $2 \times 3$ matrix)
The transpose is:
$$A^T = \begin{bmatrix} 1 & 4 \\ 2 & 5 \\ 3 & 6 \end{bmatrix}$$
(This is a $3 \times 2$ matrix)


# Tutorials
- https://www.youtube.com/watch?v=yRwQ7A6jVLk
