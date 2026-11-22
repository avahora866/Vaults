---
tags:
  - CHECKLIST
---

# Introduction to Linear Algebra
- [ ] **Definition**: The study of vectors, matrices, and linear transformations.

# Vectors
- [x] **Definition**: An ordered list of numbers, representing quantities with both magnitude and direction.
- [x] **Basic Operations**:
    - [x] **Addition**: Adding corresponding components.
    - [x] **Scalar Multiplication**: Multiplying each component by a scalar.
- [x] **Dot Product**: .
- [x] **Cross Product (3D vectors)**: Useful for computing areas and normal vectors.
- [x] **Linear Independence**: A set of vectors is linearly independent if no vector in the set can be written as a combination of others.
- [ ] **Basis and Dimension**: A set of linearly independent vectors that span a vector space.

# Matrices
- [x] **Definition**: A rectangular array of numbers arranged in rows and columns.
- [x] **Types of Matrices**:
    - [x] **Square Matrix**: Same number of rows and columns.
    - [x] **Identity Matrix**: Diagonal matrix with ones on the diagonal.
    - [x] **Diagonal Matrix**: A square matrix with non- [ ]zero entries only on the diagonal.
    - [x] **Transpose**: The matrix formed by swapping rows and columns.
- [x] **Matrix Operations**:
    - [x] **Addition**: Add corresponding elements.
    - [x] **Scalar Multiplication**: Multiply each element by a scalar.
    - [x] **Matrix Multiplication**: Multiply matrices using row- [ ]by- [ ]column method.
    - [x] **Inverse**: A matrix that, when multiplied by the original matrix, results in the identity matrix (only for non- [ ]singular matrices).
    - [x] **Determinant**: A scalar value that can be computed from a square matrix and used to check if a matrix is invertible.
- [ ] **Matrix Transformations**:
    - [ ] **Linear Transformations**: Functions that map vectors to other vectors while preserving vector addition and scalar multiplication.

# Linear Systems
- [ ] **System of Linear Equations**: Represented in matrix form as .
- [ ] **Gaussian Elimination**: A method for solving systems of linear equations.
- [ ] **Row Echelon Form**: A matrix form used in Gaussian elimination to solve linear systems.
- [ ] **Homogeneous Systems**: Systems where .

# **Eigenvalues and Eigenvectors**
- [ ] **Eigenvalue**: Scalar that satisfies , where is the eigenvector.
- [ ] **Finding Eigenvalues**: Solve the characteristic equation .
- [ ] **Diagonalization**: Process of finding a diagonal matrix such that , where is the matrix of eigenvectors.

# Basics of Vector Spaces
- [ ] **Definition**: A collection of vectors that can be added together and multiplied by scalars while satisfying certain properties.
- [ ] **Subspaces**: A subset of a vector space that is also a vector space.
- [ ] **Span**: The set of all linear combinations of a set of vectors.

# Orthogonality
- [ ] **Orthogonal Vectors**: Vectors are orthogonal if their dot product is zero.
- [ ] **Orthogonal Matrices**: Matrices whose rows and columns are orthogonal unit vectors.
- [ ] **Gram- [ ]Schmidt Process**: A method for converting a set of vectors into an orthonormal set.

# Coordinate Systems and Change of Basis
- [ ] **Coordinate Systems**: Understanding how to express vectors in different bases.
- [ ] **Change of Basis**: Transforming a vector from one basis to another using a change of basis matrix.

# Linear Transformations and Their Matrix Representation
- [ ] **Linear Transformation**: A function that maps vectors to other vectors in a linear fashion (preserving vector addition and scalar multiplication).
- [ ] **Matrix Representation of Linear Transformations**: Any linear transformation can be represented by a matrix.

# Norms and Inner Products
- [ ] **Norm of a Vector**: A measure of the vector's length, typically calculated as .
- [ ] **Inner Product**: A generalization of the dot product in higher dimensions and different vector spaces.

# QR Decomposition
- [ ] **QR Factorization**: Decomposing a matrix into the product of an orthogonal matrix and an upper triangular matrix . Useful in solving least squares problems.

# Cholesky Decomposition
- [ ] **Cholesky Decomposition**: Factorizes a symmetric positive-definite matrix into a product of a lower triangular matrix and its transpose.

# Special Matrices
- [ ] **Symmetric Matrix**: .
- [ ] **Skew-Symmetric Matrix**: .
- [ ] **Sparse Matrices**: Matrices with a lot of zero entries.