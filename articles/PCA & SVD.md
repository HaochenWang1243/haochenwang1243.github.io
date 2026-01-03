# Principal Component Analysis and its relation to SVD

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

## 5. Multiple Principal Components (Lagrange multiplier derivation)

Instead of imposing orthogonality by hand, we maximize **total variance in two directions at once**:

$$
\max_{u_1, u_2} \quad u_1^T C u_1 + u_2^T C u_2
$$

subject to unit-length constraints:

$$
u_1^T u_1 = 1, \qquad u_2^T u_2 = 1
$$

Form the Lagrangian:

$$
\mathcal{L} = 
u_1^T C u_1 + u_2^T C u_2
- \lambda_1 (u_1^T u_1 - 1)
- \lambda_2 (u_2^T u_2 - 1)
$$

Take derivatives:

$$
\frac{\partial \mathcal{L}}{\partial u_1} = 2 C u_1 - 2\lambda_1 u_1 = 0
\quad \Rightarrow \quad
C u_1 = \lambda_1 u_1
$$

$$
\frac{\partial \mathcal{L}}{\partial u_2} = 2 C u_2 - 2\lambda_2 u_2 = 0
\quad \Rightarrow \quad
C u_2 = \lambda_2 u_2
$$

Thus:

- $u_1$ and $u_2$ are eigenvectors of $C$
- Ordering eigenvalues gives:

$$
\lambda_1 \ge \lambda_2
$$

Because $C$ is symmetric, eigenvectors associated with distinct eigenvalues are automatically orthogonal, so:

$$
u_1^T u_2 = 0
$$

Orthogonality is therefore **not imposed — it emerges from the optimization**.

---

## 6. Geometric justification of orthogonality

Consider projecting data onto the 2-D subspace spanned by two vectors:

$$
\hat{x} = (u_1 u_1^T + u_2 u_2^T)x
$$

If $u_2$ is not orthogonal to $u_1$, then part of the variance explained by $u_2$ lies in the same direction as $u_1$.

That means we:

- double-count variance
- fail to expand the spread of data into a new dimension
- waste the second component

### 6.1 Decomposition of variance

Take any candidate direction $u$ and decompose it into parts parallel and orthogonal to $u_1$:

$$
u = \alpha u_1 + u_{\perp}, \qquad u_1^T u_{\perp} = 0.
$$

Then

$$
u^T C u = \alpha^2 \lambda_1 + u_{\perp}^T C u_{\perp}.
$$

The first term $\alpha^2 \lambda\_1$ is simply variance **already captured by $u\_1$**.

To genuinely add new variance, we should maximize only the orthogonal part:

$$
u\_{\perp}^T C u\_{\perp}.
$$

This expression is maximized when $u\_{\perp}$ is the eigenvector associated with the second-largest eigenvalue.

---

## 7. Projection onto Principal Components

For dimension reduction to $k$ dimensions:

$$
U_k = [u_1, u_2, \dots, u_k] \in \mathbb{R}^{d \times k}
$$

Project data:

$$
Y = U_k^T \tilde{X} \in \mathbb{R}^{k \times n}
$$

Each column of $Y$ is the representation of the original sample in the top $k$ PCA components.

### Terminology Notice: $\mathbb{R}^d \rightarrow \text{subspace in } \mathbb{R}^d$ projection vs $\mathbb{R}^d \rightarrow \mathbb{R}^k$ projection     
The PCA dimensionality reduction achieved by computing 

$$
Y = U_k^T \tilde{X} \in \mathbb{R}^{k \times n},
$$

gives the **coordinate vectors** of the data projected onto the k-dimensional space spanned by principal components. This is a $\mathbb{R}^d \rightarrow \mathbb{R}^k$ trasnformation. 

On the other hand,  

$$
\hat{X} = U_k U_k^T \tilde{X} \in \mathbb{R}^{d \times n},
$$

is the orthogonal projection of the vectors in the data matrix, which belong to $\mathbb{R}^d$, to the k-dimensional linear subspace $S \subset \mathbb{R}^d$ spanned by the PCs. This is a geometrical orthogonal projection where one draws a straight line from $\mathbb{R}^d$ to $S$. This is a $\mathbb{R}^d \rightarrow S$ trasnformation that linearly combines the basis vectors in $U_k$ by the coordinates $Y = U_k^T \tilde{X}$, and can be seen as a low-dimensional "reconstruction" of $\tilde{X}$.

---

## 8. Connection to SVD

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

## 9. Equivalence Between PCA Reconstruction and SVD Low-Rank Approximation

### 9.1 PCA Reconstruction Formula

Given a centered data vector $x \in \mathbb{R}^d$ (a column of $\tilde{X}$), its reconstruction using the top $k$ principal components is:

$$
\hat{x} = U_k (U_k^T x) = \sum_{i=1}^k (u_i^T x) u_i
$$

where $U_k = [u_1, \dots, u_k] \in \mathbb{R}^{d \times k}$ contains the first $k$ eigenvectors of $C$.

**Interpretation:**
- $u_i^T x$ is the projection coefficient (score) onto the $i$-th PC
- $\sum_{i=1}^k (u_i^T x) u_i$ reconstructs $x$ using only the top $k$ components
- The reconstruction error is $x - \hat{x} = \sum_{i=k+1}^d (u_i^T x) u_i$

---

### 9.2 SVD Low-Rank Approximation

Recall the SVD of the centered data matrix:

$$
\tilde{X} = U \Sigma V^T = \sum_{i=1}^r \sigma_i u_i v_i^T
$$

where $r = \text{rank}(\tilde{X})$, $U \in \mathbb{R}^{d \times r}$, $\Sigma \in \mathbb{R}^{r \times r}$, $V \in \mathbb{R}^{n \times r}$.

The **best rank-$k$ approximation** (in Frobenius norm) to $\tilde{X}$ is:

$$
\tilde{X}_k = \sum_{i=1}^k \sigma_i u_i v_i^T = U_k \Sigma_k V_k^T
$$

where $\Sigma_k = \text{diag}(\sigma_1, \dots, \sigma_k)$, $V_k = [v_1, \dots, v_k]$.

---

### 9.3 Connection for Individual Data Points

Consider the $j$-th centered data point $x_j$ (the $j$-th column of $\tilde{X}$). From SVD:

$$
x_j = \tilde{X} e_j = U \Sigma V^T e_j = \sum_{i=1}^r \sigma_i u_i (v_i^T e_j)
$$

where $e_j$ is the $j$-th standard basis vector in $\mathbb{R}^n$.

The rank-$k$ approximation gives:

$$
\hat{x}_j^{(SVD)} = \tilde{X}_k e_j = \sum_{i=1}^k \sigma_i u_i (v_i^T e_j)
$$

---

### 9.4 Equivalence Proof

From PCA reconstruction:

$$
\hat{x}_j^{(PCA)} = U_k (U_k^T x_j) = \sum_{i=1}^k (u_i^T x_j) u_i
$$

From SVD, note that:

$$
u_i^T x_j = u_i^T (\tilde{X} e_j) = u_i^T (U \Sigma V^T e_j) = e_i^T \Sigma V^T e_j = \sigma_i v_i^T e_j
$$

because $U$ has orthonormal columns: $u_i^T U = e_i^T$ where $e_i$ is the $i$-th standard basis vector.

Therefore:

$$
\hat{x}_j^{(PCA)} = \sum_{i=1}^k (\sigma_i v_i^T e_j) u_i = \sum_{i=1}^k \sigma_i u_i (v_i^T e_j) = \hat{x}_j^{(SVD)}
$$

---

### 9.5 Matrix Form Equivalence

For all data points simultaneously:

**PCA reconstruction:**

$$
\hat{X}^{(PCA)} = U_k U_k^T \tilde{X}
$$

**SVD rank-$k$ approximation:**

$$
\hat{X}^{(SVD)} = U_k \Sigma_k V_k^T
$$

But from SVD: $\tilde{X} = U \Sigma V^T$, so:

$$
U_k^T \tilde{X} = U_k^T U \Sigma V^T = [I_k \ 0] \Sigma V^T = \Sigma_k V_k^T
$$

Therefore:

$$
U_k U_k^T \tilde{X} = U_k (\Sigma_k V_k^T) = U_k \Sigma_k V_k^T = \hat{X}^{(SVD)}
$$

---

### 9.6 Geometric Interpretation

The operation $U_k U_k^T$ is the **orthogonal projection** onto the subspace spanned by the top $k$ principal components. This projection:

1. **Preserves** components in the PCA subspace
2. **Discards** components orthogonal to it
3. **Minimizes** reconstruction error $\|\tilde{X} - \hat{X}\|_F^2$ among all rank-$k$ matrices

The squared reconstruction error per sample is:

$$
\mathbb{E}[\|x - \hat{x}\|^2] = \sum_{i=k+1}^d \lambda_i
$$

where $\lambda_i$ are the eigenvalues of $C$ (variances along discarded PCs).

---

### 9.7 Summary of Equivalence

| Concept | PCA Formulation | SVD Formulation | Equivalence |
|---------|-----------------|-----------------|-------------|
| **Subspace** | Span of $u_1, \dots, u_k$ | Span of first $k$ left singular vectors | $U_k$ (same) |
| **Projection** | $P = U_k U_k^T$ | $P = U_k U_k^T$ | Identical |
| **Coefficients** | $U_k^T x$ | $\Sigma_k V_k^T$ columns | Related by scaling |
| **Reconstruction** | $\hat{x} = U_k U_k^T x$ | $\hat{x} = \sum_{i=1}^k \sigma_i (v_i^T e_j) u_i$ | Identical |
| **Matrix approx** | $\hat{X} = U_k U_k^T \tilde{X}$ | $\hat{X} = U_k \Sigma_k V_k^T$ | $U_k U_k^T \tilde{X} = U_k \Sigma_k V_k^T$ |

---

## 10. Eckart–Young–Mirsky Theorem

**Statement**:  
Let $X \in \mathbb{R}^{m \times n}$ with singular value decomposition $X = U \Sigma V^T$, where  
$\Sigma = \text{diag}(\sigma_1, \sigma_2, \dots, \sigma_p)$, $\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_p \geq 0 $, and $p = \min(m,n)$.  
Define the rank-$k$ approximation $X_k = U \Sigma_k V^T$, where $\Sigma_k$ is $\Sigma$ with $\sigma_{k+1}, \dots, \sigma_p$ set to 0.

Then for any rank-$r$ matrix $B$ with $r \leq k < \text{rank}(X)$:

$$
\| X - X\_k \| \leq \| X - B \|
$$

for **both** the Frobenius norm $\|\cdot\|\_F$ and the spectral norm $\|\cdot\|\_2$
Moreover, $X\_k$ is a minimizer (not necessarily unique if $\sigma_k = \sigma_{k+1}$).


**Key Insight:** PCA reconstruction $U\_k U\_k^T x$ is exactly the orthogonal projection of $x$ onto the principal subspace, which is mathematically equivalent to the rank- $k$ approximation provided by truncated SVD. Both SVD low-rank approximation and PCA reconstruction are orthogonal projections of vectors in the data matrix onto the optimal low-dimensional subspace, where "optimal" means minimizing the reconstruction error in Frobenius/ℓ² norm over the whole data matrix. This optimal subspace is spanned by the (orthogonal) basis formed by columns of $U_k$

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
