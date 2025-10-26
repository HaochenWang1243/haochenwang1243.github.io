# Signal and Noise Power in Communication Systems

---

## 1. Deterministic Signal

For a deterministic signal \(s(t)\), its power is defined as the **time-average**:

$$
P_s = \frac{1}{T} \int_0^T |s(t)|^2 \, dt
$$

- No stochastic assumption is needed.  
- Example: For \(s(t) = A \cos(2 \pi f t)\),  

$$
P_s = \frac{A^2}{2}
$$

---

## 2. Random Signal / Stochastic Process

For a random signal \(x(t)\), we typically define power using the **expectation**:

$$
P_x = \lim_{T \to \infty} \frac{1}{T} \int_0^T \mathbb{E}[|x(t)|^2] \, dt
$$

- If the process is **Wide-Sense Stationary (WSS)**:  
  - Mean and autocorrelation are time-invariant.  
  - Time average = ensemble average, so computing power via time averaging is justified.  

- If the process is **not WSS**, the time-average of a single realization may **not equal** the expected power.

---

## 3. Summary Table

| Case | Power Calculation | Notes |
|------|-----------------|-------|
| Deterministic signal | \(P_s = \frac{1}{T}\int_0^T |s(t)|^2 dt\) | No stochastic assumptions needed |
| Random WSS signal | \(P_s = \mathbb{E}[|x(t)|^2]\) | Time average = ensemble average |
| Random non-WSS signal | Time-average may not equal expected power | Must use ensemble averaging |

---

In digital communications, it is common to calculate **signal power** in the time domain and **noise power** in the frequency domain. Here's why:

---

## 1. Noise Power

- Consider **AWGN (Additive White Gaussian Noise)** with **Power Spectral Density (PSD)** \(S_n(f)\).  
- The total noise power in a receiver of bandwidth \(B\) is:

$$
P_n = \int_{f_1}^{f_2} S_n(f) \, df \approx N_0 B
$$

- **Intuition:** Each 1 Hz of bandwidth contributes \(N_0\) watts of noise.

> Noise is naturally characterized in the **frequency domain** because we are concerned with how much power falls within the system’s bandwidth.

---

## 2. Signal Power

- Suppose the transmitted signal is deterministic \(s(t)\).  
- Its power is typically computed as the **time-average**:

$$
P_s = \frac{1}{T} \int_0^T |s(t)|^2 \, dt
$$

- **Intuition:** We are averaging the squared magnitude of the waveform over time to find how much power it carries per second.

> Signal is deterministic, so time-domain averaging is natural.

---

## 3. Connection via Parseval’s Theorem

- Signal power can also be calculated in the frequency domain using **Parseval’s relation**:

$$
P_s = \frac{1}{T} \int_0^T |s(t)|^2 \, dt = \int_{-\infty}^{\infty} |S(f)|^2 \, df
$$

- This shows that **time-domain and frequency-domain power are equivalent**, but for practical reasons we often compute noise in frequency and signal in time.

---

## 4. Summary Table

| Quantity       | Typical Calculation                         | Domain        |
|----------------|--------------------------------------------|---------------|
| Signal Power \(P_s\) | \( \frac{1}{T} \int_0^T |s(t)|^2 dt \) | Time          |
| Noise Power \(P_n\)  | \( \int S_n(f) df \approx N_0 B \)    | Frequency     |

- Both are **energy per unit time**, but the calculation domain differs for convenience and intuition.
