# **Singular Value Decomposition (SVD)**

Any matrix $\in \mathbb{R}^{n\times m}$ can be decomposed into three new matrices:

$$
SVD(A) = U\Sigma V^T\\
$$

where,

- $A$ = input n x m matrix
- $U$ = orthogonal matrix (m x m), left singular values
- $\Sigma$ = diagonal singular values matrix (m x n)
- $V$ = orthogonal matrix (n x n), right singular values


## **Orthogonal Matrix**

A matrix is orthogonal if its rows and columns are orthonormal vectors.

$$
Q^TQ = QQ^T = I, \text{where } Q^T = Q^{-1}
$$

A vector is considered orthonormal if it has a magnitude of 1 and a dot product ($\cdot$) of 0 with other vectors.

Example of a rotation matrix:

$$
R = \begin{bmatrix}
\cos \theta & -\sin \theta \\
\sin \theta & \cos \theta
\end{bmatrix}
$$

The rotation matrix is orthogonal because $R R^T = I$ and $R^T = R^{-1}$.

## **Singular Values**

Singular values are essentially the square roots of the eigenvalues ($\lambda_1, \lambda_2, \lambda_3, ..., \lambda_n$) of $A^TA$, so the singular values are:

$$
\sigma_1 = \sqrt\lambda_1, \sigma_2 = \sqrt\lambda_2, \sigma_3 = \sqrt\lambda_3, ..., \sigma_n = \sqrt\lambda_n
$$

The matrix $A^TA$ is symmetric and of size $n \times n$. The diagonal matrix $\Sigma$ contains the singular values $\sigma_n$ arranged in descending order and all are $\geq 0$. 

!!! info "Non-commutative of Matrix Multiplication"
    $AB \neq BA$, so in the context of SVD $A^TA\neq AA^T$

## **Rank Matrix**

The rank of a matrix is the number of linearly independent rows or columns.

Example:

$$
\begin{align*}
A &= \begin{bmatrix}
1&2&3\\
4&5&6\\
7&8&9\\
\end{bmatrix}, &R_2=R_2-4R_1\\
A &= \begin{bmatrix}
1&2&3\\
0&-3&-6\\
7&8&9\\
\end{bmatrix}, &R_3 = R_3-7R_1\\
A &= \begin{bmatrix}
1&2&3\\
0&-3&-6\\
0&-6&-12\\
\end{bmatrix}, &R_2 = \frac{R_2}{R_3}\\
A &= \begin{bmatrix}
1&2&3\\
0&1&2\\
0&-6&-12\\
\end{bmatrix}, &R_3=R_3+6R3\\
A &= \begin{bmatrix}
1&2&3\\
0&1&2\\
0&0&0\\
\end{bmatrix}
\end{align*}
$$

Thus, the rank of matrix $A$ is 2 (the number of non-zero rows or columns).

## **How To** 

1. Compute $A^TA$ (which is symmetric). The eigenvectors of a symmetric matrix are orthogonal. To obtain the matrix $V^T$, find the eigenvectors of $A^TA$.
2. Compute the eigenvalues ($\lambda$) by solving the equation $\text{det}(A^TA - \lambda I) = 0$, which gives the eigenvalues $\lambda_1, \lambda_2, ..., \lambda_n$.
3. Compute the eigenvectors ($v$) corresponding to the eigenvalues $\lambda_{1
}$. To ensure the eigenvectors are normalized, divide each element of the eigenvector by $\sqrt{v_1^2 + v_2^2 + \dots + v_n^2}$.
4. Compute the singular values $\sigma_i = \sqrt{\lambda_i}$.​
5. Repeat steps 1-3 with $AA^T$ to obtain the matrix $U$.