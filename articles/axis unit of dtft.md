# Discrete-Time Fourier Transform (DTFT) in Different Frequency Units

The **Discrete-Time Fourier Transform (DTFT)** represents a discrete-time signal $x[n]$ as a continuous function of frequency.  
Although the DTFT itself is conceptually the same, its **mathematical form changes depending on the unit** used for the frequency axis.

---

## 1. DTFT in Angular Frequency (radians/sample)

**Frequency variable:** $\omega$

**Unit:** radians per sample  
**Range:** $[-\pi, \pi]$  
**Period:** $2\pi$

### Equations

**Analysis (forward DTFT):**

$$
X(e^{j\omega}) = \sum_{n=-\infty}^{\infty} x[n]\, e^{-j\omega n}
$$

**Synthesis (inverse DTFT):**

$$
x[n] = \frac{1}{2\pi} \int_{-\pi}^{\pi} X(e^{j\omega})\, e^{j\omega n} \, d\omega
$$

**Interpretation:**  
The frequency axis $\omega$ measures **radians per sample**.  
The DTFT is **periodic with period $2\pi$** because discrete-time signals inherently repeat in the frequency domain.

---

## 2. DTFT in Normalized Frequency (cycles/sample)

**Frequency variable:** $F = \frac{\omega}{2\pi}$

**Unit:** cycles per sample  
**Range:** $[-\tfrac{1}{2}, \tfrac{1}{2}]$  
**Period:** $1$

### Equations
**Analysis:**

$$
X(F) = \sum_{n=-\infty}^{\infty} x[n]\, e^{-j2\pi F n}
$$

**Synthesis:**

$$
x[n] = \int_{-1/2}^{1/2} X(F)\, e^{j2\pi F n}\, dF
$$

**Interpretation:**  
The frequency axis $F$ expresses **cycles per sample**, i.e., how many oscillations occur per discrete sample.  
The DTFT repeats every **1 cycle/sample**, corresponding to $2\pi$ radians/sample.

---

## 3. DTFT in Physical Frequency (Hz)

**Frequency variable:** $f = F_s F = \frac{F_s}{2\pi}\omega$

**Unit:** Hertz (cycles per second)  
**Range:** $[-\tfrac{F_s}{2}, \tfrac{F_s}{2}]$  
**Period:** $F_s$

### Equations
**Analysis:**

$$
X(f) = \sum_{n=-\infty}^{\infty} x[n]\, e^{-j2\pi f n / F_s}
$$

**Synthesis:**

$$
x[n] = \int_{-F_s/2}^{F_s/2} X(f)\, e^{j2\pi f n / F_s}\, \frac{df}{F_s}
$$

**Interpretation:**  
The frequency axis $f$ is now in **Hertz**, tied directly to physical time via the sampling rate $F_s = 1/T$.  
The DTFT is **periodic every $F_s$ Hz**, because sampling in time replicates the spectrum in frequency with that spacing.

---

## Summary Table
<table>
<tr><th>Frequency variable</th><th>Unit</th><th>Range</th><th>Period</th><th>Forward DTFT</th><th>Inverse DTFT</th></tr>
<tr>
<td>&omega;</td>
<td>radians/sample</td>
<td>[-&pi;, &pi;]</td>
<td>2&pi;</td>
<td>&sum; x[n] e<sup>-j&omega;n</sup></td>
<td>(1/2&pi;) &int; X(e<sup>j&omega;</sup>) e<sup>j&omega;n</sup> d&omega;</td>
</tr>
<tr>
<td>F</td>
<td>cycles/sample</td>
<td>[-1/2, 1/2]</td>
<td>1</td>
<td>&sum; x[n] e<sup>-j2&pi; F n</sup></td>
<td>&int; X(F) e<sup>j2&pi; F n</sup> dF</td>
</tr>
<tr>
<td>f</td>
<td>Hz</td>
<td>[-Fs/2, Fs/2]</td>
<td>Fs</td>
<td>&sum; x[n] e<sup>-j2&pi; f n / Fs</sup></td>
<td>&int; X(f) e<sup>j2&pi; f n / Fs</sup> df / Fs</td>
</tr>
</table>


---

### Key Insights

The DTFT’s shape is **identical** under all three forms; only the **frequency scaling and normalization** change.  
Using radians/sample ($\omega$) is most common for theoretical work.  
Using Hz ($f$) makes the DTFT directly comparable to **physical signals** in the continuous-time domain.
