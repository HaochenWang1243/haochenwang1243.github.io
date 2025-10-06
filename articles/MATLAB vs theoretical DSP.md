# Finite Signals and Frequency Domain Intuition

In practice, all signals we process are **finite in duration**. Even when we generate "ideal" signals like sinusoids or sinc functions in MATLAB or Python, they are always truncated and discretely sampled. This has a direct effect in the frequency domain.

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

### 2. Consequences in Frequency Domain

| Signal | Infinite FT | Finite FT | Effect |
|--------|------------|-----------|-------|
| Sinusoid | $\delta(\omega\pm \omega_0)$ | sinc-shaped lobe at $\pm \omega_0$ | Peak spreads, side-lobes → **spectral leakage** |
| Sinc | rect | rect * sinc | Sharp edges become smooth, Gibbs phenomenon |

- **Intuition**: Time truncation → frequency smoothing  
- **Shorter signals** → wider main lobe  
- **Longer signals** → spectrum approaches ideal

---

### 3. Discrete-Time and DFT

- In MATLAB, signals are **discrete**:
```matlab
fs = 1000; t = 0:1/fs:1-1/fs; x = sin(2*pi*50*t);
