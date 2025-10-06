## 🧮 Linear Algebra Perspective of the Discrete-Time Fourier Series (DTFS)

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

### 6. Coefficient Computation (Analysis Equation)

Using the orthonormal basis, we can project $x_p[n]$ onto each basis vector:

$$
a_k = \frac{1}{N} \sum_{n=0}^{N-1} x_p[n] \, e^{-j \frac{2\pi}{N} k n}
$$

---

### ✅ Summary

| Concept | DTFS Interpretation |
|----------|----------------------|
| Vector space | $\mathbb{C}^N$, the space of $N$-periodic signals |
| Basis functions | $e^{j \frac{2\pi}{N} k n}$, $k = 0, \ldots, N-1$ |
| Inner product | $\langle x, y \rangle = \sum x[n] y^*[n]$ |
| Orthogonality | $\langle e^{j \frac{2\pi}{N} k n}, e^{j \frac{2\pi}{N} m n} \rangle = N \delta[k-m]$ |
| Orthonormal basis | $\phi_k[n] = \frac{1}{\sqrt{N}} e^{j \frac{2\pi}{N} k n}$ |

---

Hence, the DTFS is **a coordinate transformation** from the **time-domain basis** (unit impulses at each sample) to the **frequency-domain orthonormal basis** of complex exponentials.
