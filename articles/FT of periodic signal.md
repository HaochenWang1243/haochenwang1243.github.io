# 📘 Fourier Transform of Periodic Signals

## 1. Periodic Continuous-Time Signals

Let $x_p(t)$ be a **continuous-time periodic signal** with period $T$:

$$
x_p(t + T) = x_p(t), \quad \forall t
$$

Its **Fourier Series (FS)** representation is:

$$
x_p(t) = \sum_{k=-\infty}^{\infty} c_k \, e^{j 2 \pi k t / T}
$$

where

$$
c_k = \frac{1}{T} \int_0^T x_p(t) \, e^{-j 2 \pi k t / T} dt
$$

---

## 2. Fourier Transform of a Periodic Signal

The **Fourier Transform (FT)** of $x_p(t)$ is defined as:

$$
X_p(f) = \int_{-\infty}^{\infty} x_p(t) \, e^{-j 2\pi f t} dt
$$

However, for a **periodic signal**, this integral does **not converge in the usual sense** because the signal is not absolutely integrable.  

- Instead, we treat $X_p(f)$ as a **generalized function** (distribution), consisting of **impulses (Dirac delta functions)** at harmonic frequencies.

---

## 3. Connection Between FS and FT

Using the Fourier Series representation:

$$
x_p(t) = \sum_{k=-\infty}^{\infty} c_k \, e^{j 2 \pi k t / T}
$$

Take the Fourier Transform of both sides:

$$
\begin{align*}
X_p(f) &= \mathcal{F}\left\{\sum_{k=-\infty}^{\infty} c_k \, e^{j 2 \pi k t / T}\right\} \\
&= \sum_{k=-\infty}^{\infty} c_k \, \mathcal{F}\{ e^{j 2 \pi k t / T} \} \\
&= \sum_{k=-\infty}^{\infty} c_k \, \delta\Big(f - \frac{k}{T}\Big)
\end{align*}
$$

✅ The FT of a periodic signal is **a train of impulses** at the harmonic frequencies $f = k/T$, and the **impulse amplitudes are the FS coefficients $c_k$**.

---

## 4. Intuition

- A **continuous-time periodic signal** has energy spread infinitely in time, so its FT is **not a normal function** but a **series of impulses**.  
- The **FS coefficients** $c_k$ tell us the **strength of each harmonic**, and the FT just places these strengths at the corresponding frequencies.

- **FS ↔ FT relationship**:

$$
X_p(f) = \sum_{k=-\infty}^{\infty} c_k \, \delta\Big(f - \frac{k}{T}\Big), \quad c_k = \frac{1}{T} X_0(f=k/T)
$$

where $X_0(f)$ is the FT of **one period** of the signal.

---

## 5. Summary Table

| Concept | Fourier Series (FS) | Fourier Transform (FT) of a periodic signal |
|---------|--------------------|-------------------------------------------|
| Signal | Periodic $x_p(t)$ | Same periodic $x_p(t)$ |
| Spectrum | Discrete coefficients $c_k$ | Impulse train: $\sum c_k \delta(f-k/T)$ |
| Relation | $c_k = \frac{1}{T} \int_0^T x_p(t) e^{-j 2\pi k t/T} dt$ | FT = impulses at harmonics with amplitude $c_k$ |
| Intuition | Energy per harmonic | Impulse locations = harmonic frequencies, heights = FS coefficients |

---

### ✅ Key Takeaway

The **Fourier Transform of a periodic continuous-time signal** is **a train of Dirac delta functions** at harmonic frequencies. Each impulse has an amplitude equal to the corresponding **Fourier Series coefficient**.  

This provides a **bridge between FS and FT**:  

- FS = “coefficients” in time domain  
- FT = “impulse spectrum” in frequency domain
