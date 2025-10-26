# Signal and Noise Power in Communication Systems

---

## 1. Signal Power

### Deterministic Signal
For a deterministic signal $s(t)$, the power is defined as the **time-average**:

$$
P_s = \frac{1}{T} \int_0^T |s(t)|^2 \, dt
$$

- No stochastic assumption is needed.  
- Example: $s(t) = A \cos 2\pi f t \implies P_s = \frac{A^2}{2}$

### Random Signal / Stochastic Process
For a random signal $x(t)$, the power is typically defined using the **expectation**:

$$
P_x = \lim_{T \to \infty} \frac{1}{T} \int_0^T \mathbb{E} |x(t)|^2 \, dt
$$

- **Wide-Sense Stationary (WSS):** time-average = ensemble average.  
- **Non-WSS:** time-average of a single realization may **not equal** the expected power; ensemble averaging is required.

### Time vs. Frequency Domain

| Quantity       | Typical Calculation                         | Domain        |
|----------------|--------------------------------------------|---------------|
| Signal Power $P_s$ | $ \frac{1}{T} \int_0^T |s(t)|^2 dt $ | Time          |
| Noise Power $P_n$  | $ \int S_n(f) df \approx N_0 B $    | Frequency     |

- **Parseval’s theorem** connects the two domains:

$$
\frac{1}{T} \int_0^T |s(t)|^2 dt = \int_{-\infty}^{\infty} |S(f)|^2 df
$$

---

## 2. Noise Power

For **AWGN (Additive White Gaussian Noise)** with PSD $N_0/2$:

$$
P_n = \int_{f_1}^{f_2} S_n(f) \, df \approx N_0 B
$$

- Noise is often calculated in the **frequency domain**, as it is uniform across frequency.  
- Deterministic signal power is often calculated in **time domain**.  

---

## 3. Instantaneous SNR

The **instantaneous SNR** is the SNR at a specific time or channel realization:

$$
\text{SNR}_{\text{inst}}(t) = \frac{P_{\text{signal}}(t)}{P_{\text{noise}}(t)}
$$

- **Example:** Flat-fading complex Gaussian channel:

$$
r = h s + n, \quad h \sim \mathcal{CN}(0, \sigma_h^2)
$$

$$
\text{SNR}_{\text{inst}} = \frac{|h|^2 E_s}{N_0}
$$

- It is a **random variable** because the channel $h$ varies with time.  
- Critical in fading channels and adaptive communication systems.  

---

## 4. Summary

- **Signal power:** usually computed in time domain (deterministic)  
- **Noise power:** usually computed in frequency domain (AWGN)  
- **Instantaneous SNR:** varies with time or channel realization, important in fading/adaptive systems
