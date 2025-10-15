# Wiener-Khinchin Theorem via Convolution

The **Wiener-Khinchin theorem** states that the power spectral density (PSD) of a signal is the Fourier transform of its autocorrelation function. We can prove this easily by noting that the autocorrelation function is essentially a convolution.

---

## 1. Autocorrelation as Convolution

For a deterministic discrete-time signal $x[n]$, the autocorrelation function $R_{xx}[k]$ is defined as:

$$
R_{xx}[k] = \sum_{n=-\infty}^{\infty} x[n] x^*[n-k]
$$

Notice that this is exactly the **convolution** of $x[n]$ with its time-reversed conjugate $x^*[-n]$:

$$
R_{xx}[k] = (x * x^*[-n])[k]
$$

---

## 2. Fourier Transform of a Convolution

The **Fourier transform property** of convolution states that:

$$
\mathcal{F}\{f * g\} = F(\omega) \, G(\omega)
$$

Applying this to the autocorrelation:

$$
\mathcal{F}\{R_{xx}[k]\} = \mathcal{F}\{x[n] * x^* [-n]\} = X(\omega) \cdot X^* (\omega) = |X(\omega)|^2
$$

Thus, the PSD of $x[n]$ is:

$$
S_{xx}(\omega) = |X(\omega)|^2
$$

This is exactly the statement of the Wiener-Khinchin theorem.

---

## 3. Extension to Random Processes

For a **wide-sense stationary (WSS) random process** $X[n]$, the autocorrelation function is defined as:

$$
R_{XX}[k] = \mathbb{E}[X[n] X^*[n-k]]
$$

The PSD is then given by:

$$
S_{XX}(\omega) = \mathcal{F}\{R_{XX}[k]\} = \mathcal{F}\{\mathbb{E}[X[n] X^*[n-k]]\}
$$

So for random processes, the PSD differs only by **taking the expectation over the ensemble** in the autocorrelation function.

---

**Summary:**  
1. Autocorrelation is a convolution.  
2. Convolution in time corresponds to multiplication in frequency.  
3. Therefore, the PSD is the Fourier transform of the autocorrelation.  
4. For stochastic signals, we take the ensemble expectation of the autocorrelation.
