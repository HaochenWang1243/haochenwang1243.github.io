# 📘 CTFS as Samples of the CTFT

## 1. Setup: Periodic Signals

Let $x_p(t)$ be a **periodic signal** with period $T$:

$$
x_p(t + T) = x_p(t), \quad \forall t
$$

We can represent $x_p(t)$ using the **Fourier Series (FS)**:

$$
x_p(t) = \sum_{k=-\infty}^{\infty} c_k \, e^{j 2 \pi k t / T}
$$

where the **FS coefficients** are:

$$
c_k = \frac{1}{T} \int_0^T x_p(t) \, e^{-j 2 \pi k t / T} \, dt
$$

---

## 2. Fourier Transform of One Period

Let $x(t)$ denote **one period** of $x_p(t)$, defined on $[0,T]$, and zero elsewhere:

$$
x(t) =
\begin{cases}
x_p(t), & 0 \le t < T \\
0, & \text{otherwise}
\end{cases}
$$

The **Fourier Transform (FT)** of $x(t)$ is:

$$
X(f) = \int_{-\infty}^{\infty} x(t) \, e^{-j 2\pi f t} dt
= \int_0^T x_p(t) \, e^{-j 2\pi f t} dt
$$

---

## 3. FS Coefficients as Samples of FT

Compare $c_k$ with $X(f)$ evaluated at discrete frequencies $f = \frac{k}{T}$:

$$
c_k = \frac{1}{T} \int_0^T x_p(t) \, e^{-j 2 \pi k t / T} dt
= \frac{1}{T} X\Big(f = \frac{k}{T}\Big)
$$

✅ This shows that **each Fourier Series coefficient is a sample of the Fourier Transform of one period**, scaled by $1/T$.  

- FS coefficients correspond to **discrete spectral lines** at harmonics of the fundamental frequency $f_0 = 1/T$.  
- The FT of a finite-duration segment produces a **continuous spectrum**, and FS just **samples it at $f = k/T$**.

---

## 4. Intuition

- **Fourier Series**: Periodic signals $\rightarrow$ discrete frequencies $k/T$  
- **Fourier Transform of one period**: Finite-length signal $\rightarrow$ continuous frequency spectrum  
- FS coefficients = scaled **samples of this continuous spectrum** at the harmonics of the fundamental frequency  

$$
c_k = \frac{1}{T} X\Big(\frac{k}{T}\Big), \quad k \in \mathbb{Z}
$$

---

## 5. Summary Table

| Concept | FS | FT of one period |
|---------|----|----------------|
| Signal | Periodic | One period, zero elsewhere |
| Spectrum | Discrete lines at $k/T$ | Continuous |
| Coefficients | $c_k = \frac{1}{T} \int_0^T x_p(t) e^{-j 2\pi k t/T} dt$ | $X(f) = \int_0^T x_p(t) e^{-j 2\pi f t} dt$ |
| Relation | $c_k = \frac{1}{T} X(f=k/T)$ | FS = sampled FT |

# 📘 DTFS as Samples of the DTFT

## 1. Setup: Periodic Discrete-Time Signals

Let $x[n]$ be a **discrete-time periodic signal** with period $N$:

$$
x[n + N] = x[n], \quad \forall n
$$

Its **Discrete-Time Fourier Series (DTFS)** representation is:

$$
x[n] = \sum_{k=0}^{N-1} a_k \, e^{j \frac{2\pi}{N} k n}
$$

where the **DTFS coefficients** are:

$$
a_k = \frac{1}{N} \sum_{n=0}^{N-1} x[n] \, e^{-j \frac{2\pi}{N} k n}, \quad k = 0, 1, \dots, N-1
$$

---

## 2. Discrete-Time Fourier Transform (DTFT) of One Period

Let $x_0[n]$ denote **one period** of $x[n]$, i.e.,

$$
x_0[n] =
\begin{cases}
x[n], & 0 \le n \le N-1 \\
0, & \text{otherwise}
\end{cases}
$$

The **Discrete-Time Fourier Transform (DTFT)** of $x_0[n]$ is:

$$
X(e^{j\omega}) = \sum_{n=-\infty}^{\infty} x_0[n] \, e^{-j \omega n}
= \sum_{n=0}^{N-1} x[n] \, e^{-j \omega n}, \quad \omega \in [-\pi, \pi]
$$

---

## 3. DTFS Coefficients as Samples of DTFT

The DTFS coefficient $a_k$ is:

$$
a_k = \frac{1}{N} \sum_{n=0}^{N-1} x[n] \, e^{-j \frac{2\pi}{N} k n}
= \frac{1}{N} X\Big(e^{j \omega} \Big)\Big|_{\omega = 2\pi k / N}
$$

✅ This shows that **each DTFS coefficient is a sample of the DTFT of one period**, scaled by $1/N$.  

- DTFS gives **discrete spectral lines** at $\omega = 2\pi k / N$.  
- DTFT of one period gives a **continuous spectrum**, and DTFS samples it at the **harmonics of the fundamental frequency**.

---

## 4. Intuition

- **DTFS**: Periodic discrete-time signals $\rightarrow$ discrete frequencies $\omega = 2\pi k / N$  
- **DTFT of one period**: Finite-length sequence $\rightarrow$ continuous frequency spectrum over $[-\pi, \pi]$  
- DTFS coefficients = scaled **samples of DTFT** at the fundamental harmonics  

$$
\boxed{a_k = \frac{1}{N} X(e^{j 2\pi k / N}), \quad k = 0, 1, \dots, N-1}
$$

---

## 5. Summary Table

| Concept | DTFS | DTFT of one period |
|---------|------|-----------------|
| Signal | Periodic, length $N$ | One period of length $N$, zero elsewhere |
| Spectrum | Discrete lines at $\omega = 2\pi k / N$ | Continuous over $\omega \in [-\pi, \pi]$ |
| Coefficients | $a_k = \frac{1}{N} \sum x[n] e^{-j 2\pi k n / N}$ | $X(e^{j\omega}) = \sum x[n] e^{-j \omega n}$ |
| Relation | $a_k = \frac{1}{N} X(e^{j 2\pi k / N})$ | DTFS = sampled DTFT |


---

### ✅ Key Takeaway

The **Fourier Series is essentially a sampled version of the Fourier Transform** of one period of a periodic signal. This perspective connects **discrete spectral lines** (FS) with the **continuous spectrum** (FT).
