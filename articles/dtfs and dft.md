## 🧮 Linear Algebra Perspective of the Discrete-Time Fourier Series (DTFS) and Discrete Fourier Transform (DFT)

### 1. Setup

Suppose $x_p[n]$ is a discrete-time periodic signal with period $N$:

$$
x_p[n + N] = x_p[n], \quad \forall n
$$

We can represent $x_p[n]$ as a linear combination of **complex exponential basis vectors**:

$$
x_p[n] = \sum_{k=0}^{N-1} a_k \, e^{j \frac{2\pi}{N} k n}
$$

---

### 2. Vector Space Interpretation

The set of all $N$-periodic discrete-time signals forms an $N$-dimensional complex vector space.

Each function $e^{j \frac{2\pi}{N} k n}$ for $k = 0, 1, \ldots, N-1$ acts as a **basis vector** in this space.

We define an **inner product** between two signals $x[n]$ and $y[n]$ as:

$$
\langle x, y \rangle = \sum_{n=0}^{N-1} x[n] \, y^*[n]
$$

where $^*$ denotes complex conjugation.

---

### 3. Orthonormal Basis Property

We can verify that the exponential basis functions are **orthogonal** under this inner product:

$$
\langle e^{j \frac{2\pi}{N} k n}, e^{j \frac{2\pi}{N} m n} \rangle
= \sum_{n=0}^{N-1} e^{j \frac{2\pi}{N} (k - m) n}
$$

---

### 4. Proof of Orthogonality

Let $r = k - m$. Then:

$$
\sum_{n=0}^{N-1} e^{j \frac{2\pi}{N} r n} =
\text{ } N \text{ if } r = 0, \text{ and } 0 \text{ otherwise.}
$$

**Proof:**  

When $r \neq 0$:

$$
\sum_{n=0}^{N-1} e^{j \frac{2\pi}{N} r n}
= \frac{1 - e^{j 2\pi r}}{1 - e^{j \frac{2\pi}{N} r}}
= \frac{1 - 1}{1 - e^{j \frac{2\pi}{N} r}} = 0
$$

since $e^{j 2\pi r} = 1$ for any integer $r$.

When $r = 0$, each exponential term equals 1, so the sum equals $N$.

Thus:

$$
\langle e^{j \frac{2\pi}{N} k n}, e^{j \frac{2\pi}{N} m n} \rangle = N \, \delta[k - m]
$$

---

### 5. Normalization

To make the basis **orthonormal**, we divide each exponential by $\sqrt{N}$:

$$
\phi_k[n] = \frac{1}{\sqrt{N}} e^{j \frac{2\pi}{N} k n}
$$

Then:

$$
\langle \phi_k, \phi_m \rangle = \delta[k - m]
$$

---

### 6. Spanning Set Proof

To show that $\{\phi_k[n]\}_{k=0}^{N-1}$ spans the space:

1. **Linear independence:** Suppose

$$
\sum_{k=0}^{N-1} c_k \phi_k[n] = 0 \quad \forall n = 0, \dots, N-1.
$$

Take the inner product with $\phi_m[n]$:

$$
\left\langle \sum_{k=0}^{N-1} c_k \phi_k, \phi_m \right\rangle
= \sum_{k=0}^{N-1} c_k \langle \phi_k, \phi_m \rangle
= \sum_{k=0}^{N-1} c_k \delta[k - m]
= c_m = 0
$$

for all $m$. So all $c_k = 0$, proving **linear independence**.

2. **Dimension argument:** The space of $N$-periodic signals is $N$-dimensional, and we have $N$ linearly independent vectors.  

✅ By a standard linear algebra fact, **$N$ independent vectors in an $N$-dimensional space automatically span the space**.  

Hence, any $N$-periodic signal can be expressed as a linear combination of $\phi_k[n]$.

---

### 7. Coefficient Computation (Analysis Equation)

Using the orthonormal basis, we can project $x_p[n]$ onto each basis vector:

$$
a_k = \frac{1}{N} \sum_{n=0}^{N-1} x_p[n] \, e^{-j \frac{2\pi}{N} k n}
$$

---

### 8. DTFS ↔ DFT

The **Discrete Fourier Transform (DFT)** is **numerically identical to the DTFS** for a finite-length sequence treated as **one period of a periodic signal**.  

- Let $x[n]$ be a finite-length sequence of length $N$.  
- Treat it as periodic with period $N$.  
- The DFT computes the same coefficients as DTFS:

$$
X[k] = \sum_{n=0}^{N-1} x[n] \, e^{-j \frac{2\pi}{N} k n}, \quad k = 0, \dots, N-1
$$

$$
x[n] = \frac{1}{N} \sum_{k=0}^{N-1} X[k] \, e^{j \frac{2\pi}{N} k n}, \quad n = 0, \dots, N-1
$$

- The **orthonormal basis vectors** are the same:

$$
\phi_k[n] = \frac{1}{\sqrt{N}} e^{j \frac{2\pi}{N} k n}
$$

- In **matrix form**:

$$
\mathbf{X} = \mathbf{F} \mathbf{x}, \quad 
\mathbf{F}[k,n] = e^{-j 2 \pi k n / N}, \quad
\mathbf{x} = \frac{1}{N} \mathbf{F}^H \mathbf{X}
$$

where $\mathbf{F}^H$ is the conjugate transpose of $\mathbf{F}$.
