# Principal Component Analysis (PCA) - Math Explanation

This document explains PCA mathematically, step by step, including the connection to SVD and how multiple principal components are derived.

---

## 1. Data Matrix

Let the data matrix be:

$$
X \in \mathbb{R}^{d \times n}
$$

where:

* $d$ is the feature dimension
* $n$ is the number of samples

Assume $X$ is **centered** along features (columns).

---

## 2. Centering the Data

Compute the mean of each row (feature):

$$
\mu = \frac{1}{n} \sum_{i=1}^n X[:,i] \in \mathbb{R}^d
$$

Center the data:

$$
\tilde{X} = X - \mu \mathbf{1}^T
$$

where $\mathbf{1} \in \mathbb{R}^{n}$ is a vector of ones.

---

## 3. Covariance Matrix

The sample covariance matrix is:

$$
C = \frac{1}{n} \tilde{X} \tilde{X}^T \in \mathbb{R}^{d \times d}
$$

* $C$ is symmetric and positive semidefinite.

---

## 4. Eigen-decomposition (PCA directions)

PCA finds directions $u \in \mathbb{R}^d$ that maximize the variance:

$$
\text{maximize } \mathrm{Var}(u^T \tilde{X}) = u^T C u \quad \text{s.t. } |u|_2 = 1
$$

Solution: **eigenvectors of $C$**:

$$
C u_i = \lambda_i u_i
$$

* $\lambda_1 \ge \lambda_2 \ge \dots \ge \lambda_d \ge 0$
* $u_i$ are the principal directions.

---

## 5. Projection onto Principal Components

For dimension reduction to $k$ dimensions:

$$
U_k = [u_1, u_2, \dots, u_k] \in \mathbb{R}^{d \times k}
$$

Project data:

$$
Y = U_k^T \tilde{X} \in \mathbb{R}^{k \times n}
$$

Each column of $Y$ is the representation of the original sample in the top $k$ PCA components.

---

## 6. Multiple Principal Components

### First component

Solve unconstrained:

$$
\max_{|u|=1} u^T C u
$$

Gives $u_1$, eigenvector with largest eigenvalue $\lambda_1$.

### Second component

Maximize remaining variance **orthogonal** to $u_1$:

$$
\max_{|u_2|=1, u_2^T u_1=0} u_2^T C u_2
$$

This yields $u_2$, eigenvector with second-largest eigenvalue $\lambda_2$.

### Recursive formulation

For the $k$-th component:

$$
\max_{|u_k|=1, u_k^T u_i = 0, i=1,\dots,k-1} u_k^T C u_k
$$

* Sequential variance maximization subject to orthogonality.

---

## 7. Residual Variance Explanation

Residual covariance after removing first component:

$$
C_{\text{res}} = C - \lambda_1 u_1 u_1^T
$$

Residual variance of any unit vector $u$:

$$
u^T C_{\text{res}} u = u^T C u - \lambda_1 (u^T u_1)^2
$$

* Non-orthogonal vectors are penalized automatically.
* Maximizing residual variance enforces orthogonality implicitly.

---

## 8. Orthogonality of Eigenvectors

Covariance matrix $C$ is symmetric:

$$
C^T = C
$$

Eigenvectors corresponding to distinct eigenvalues are orthogonal:

$$
C u_1 = \lambda_1 u_1, \quad C u_2 = \lambda_2 u_2, \quad \lambda_1 \neq \lambda_2 \implies u_1^T u_2 = 0
$$

For repeated eigenvalues, an orthonormal basis can be constructed within the eigenspace. Hence **PCA eigenvectors are orthogonal**.

---

## 9. Connection to SVD

Take the SVD of centered data:

$$
\tilde{X} = U \Sigma V^T
$$

Then covariance matrix:

$$
C = \frac{1}{n} \tilde{X} \tilde{X}^T = U \frac{\Sigma^2}{n} U^T
$$

* Eigenvectors of $C$ = columns of $U$ (left singular vectors)
* Eigenvalues of $C$ = $\lambda_i = \sigma_i^2 / n$

PCA projections via SVD:

$$
Y = U_k^T \tilde{X} = \Sigma_k V_k^T
$$

* When $d \gg n$, SVD avoids computing $C$ explicitly.

---

### Summary Table

| Concept        | PCA                                   | SVD                       |
| -------------- | ------------------------------------- | ------------------------- |
| Data           | $\tilde X$                            | $\tilde X$                |
| Covariance     | $C = \frac{1}{n} \tilde X \tilde X^T$ | —                         |
| Directions     | Eigenvectors of $C$                   | Left singular vectors $U$ |
| Variances      | Eigenvalues $\lambda_i$               | $\sigma_i^2 / n$          |
| Projected Data | $U_k^T \tilde X$                      | $\Sigma_k V_k^T$          |

---

### Key Takeaway

PCA is equivalent to performing SVD on the centered data matrix. Eigenvectors of the covariance matrix correspond to left singular vectors of $	ilde X$, and eigenvalues correspond to squared singular values divided by $n$.
