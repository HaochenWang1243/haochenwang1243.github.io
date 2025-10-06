# DFT on finite Signals: Frequency Domain Intuition

In practice, all signals we process are **finite in duration**. Even when we generate "ideal" signals like sinusoids or sinc functions in MATLAB or Python, they are always truncated and discretely sampled. This has a direct effect in the frequency domain.

We have 2 limitations in practice compared to ideal "**DTFT on infinite duration signal**":
1. We can only model finite-duration signal
2. DTFT is continuous, we can't store it.

Refer to [FS as samples of FT of one period.md](https://github.com/HaochenWang1243/haochenwang1243.github.io/blob/main/articles/FS%20as%20samples%20of%20FT%20of%20one%20period.md#-ctfs-as-samples-of-the-ctft) for DFT relation to DTFT.

#### Now, the approximation for "**DTFT of infinite duration signal**" is "**DFT of finite-duration signal**", and they are related as follows:  
### DFT of finite-duration signal = samples of (DTFT of (infinite duration signal that is formed by 0-padding the finite-duration signal)).    
---

## 1. Infinite vs Finite Signals

- **Infinite sinusoid**:

$$
x_\infty(t) = \sin(\omega_0 t), \quad t \in (-\infty, \infty)
$$
- **Infinite sinc**:

$$
\mathrm{sinc}(t) = \frac{\sin(\pi t)}{\pi t}, \quad t \in (-\infty, \infty)
$$

Their Fourier Transforms (FTs) are idealized:

- Infinite sinusoid → delta function at $\pm \omega_0$:

$$
X_\infty(\omega) = \pi \left[\delta(\omega - \omega_0) - \delta(\omega + \omega_0)\right]
$$
- Infinite sinc → rectangular spectrum:
  
$$
\mathrm{FT}\{\mathrm{sinc}(t)\} = \mathrm{rect}\left(\frac{\omega}{2\pi}\right)
$$

---

- **Finite signals** are obtained by multiplying the ideal signal with a **window**:

$$
x_\text{finite}(t) = x_\infty(t) \cdot w(t)
$$

where $w(t)$ is typically a rectangular window of duration $T$.

- **Convolution in frequency**: Multiplication in time corresponds to convolution in frequency:

$$
X_\text{finite}(\omega) = X_\infty(\omega) * W(\omega)
$$

where $W(\omega)$ is the FT of the window (e.g., $\mathrm{sinc}$ for a rectangular window).

---
As a result,
| Signal | Infinite FT | Finite FT | Effect |
|--------|------------|-----------|-------|
| Sinusoid | $\delta(\omega\pm \omega_0)$ | sinc-shaped lobe at $\pm \omega_0$ | Peak spreads, side-lobes → **spectral leakage** |
| Sinc | rect | rect * sinc | Sharp edges become smooth, Gibbs phenomenon |

- **Intuition**: Time truncation → frequency smoothing  
- **Shorter signals** → wider main lobe  
- **Longer signals** → spectrum approaches ideal

---

### 2. DFT samples the DTFT of finite-duration signal

- In MATLAB, signals are **discrete**:

```matlab
fs = 1000; 
t = 0:1/fs:1-1/fs; 
x = sin(2*pi*50*t);
````

* The **periodogram** is:

$$
P[k] = \frac{1}{N} \left| \sum_{n=0}^{N-1} x[n] e^{-j 2 \pi k n / N} \right|^2
$$
  
  which is **the squared magnitude of the DFT**.

* The DFT **samples the DTFT** of the finite signal at discrete frequency bins.

* Therefore, the periodogram is essentially:

$$
\text{smoothed + sampled version of the infinite-duration sine spectrum.}
$$

---

### 3. Effect of 0-padding sequnces in MATLAB on DFT
Remember that the DFT is obtained by **sampling the DTFT** of the finite signal zero-padded to infinity.  
Obviously, zero-padding the finite signal itself doesn’t change its DTFT, since the DTFT is taken on a signal already zero-padded to infinity.  

What zero-padding the sequence does is simply **decrease the frequency bin width** $\frac{F_s}{N}$ by increasing $N$.  
While the (DTFT sampling) resolution of the DFT is increased, the **shape of the DTFT being sampled doesn’t change**.  

As a result, the **DTFT smoothing introduced by truncation isn’t improved** at all by zero-padding the sequence.


---

### 4. Summary Intuition

$$
\text{Infinite continuous sine} \quad \xrightarrow{\text{Truncate and Discretize}} \text{Finite discrete sine} \quad \xrightarrow{\text{DTFT}} \text{sinc-smoothed spectrum (from truncation)} \quad \xrightarrow{\text{DFT/periodogram}} \text{sampled spectrum (from discretization)}
$$

> In practice, we can still use DTFT-based intuition and sanity checks. The main difference is the **smoothing (convolution with sinc)** caused by finite duration, and the **sampling** of the spectrum by the DFT.


