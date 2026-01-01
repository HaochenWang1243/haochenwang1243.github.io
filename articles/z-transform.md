# Z-Transform, Transfer Functions, and Filter Stability

## 1. Z-Transform Formula

The **z-transform** of a discrete-time signal $x[n]$ is defined as:

$$
X(z) = \sum_{n=-\infty}^{\infty} x[n] z^{-n}
$$

where $z \in \mathbb{C}$ is a complex variable.

---

## 2. Inverse Z-Transform Formula

The **inverse z-transform** is given by the contour integral:

$$
x[n] = \frac{1}{2\pi j} \oint_C X(z) z^{n-1} dz
$$

where $C$ is a closed contour in the region of convergence (ROC) of $X(z)$.

---

## 3. Rational Polynomial Transfer Function

A **rational polynomial (RP) transfer function** $H(z)$ can be expressed as:

$$
H(z) = \frac{Y(z)}{X(z)} = \frac{b_0 + b_1 z^{-1} + \dots + b_M z^{-M}}{1 + a_1 z^{-1} + \dots + a_N z^{-N}}
$$

This can **always be decomposed into a summation of IIR filter transfer functions** via partial fraction expansion:

$$
H(z) = \sum_{k=1}^{N} \frac{R_k}{1 - p_k z^{-1}}
$$

where $p_k$ are the poles and $R_k$ are the residues corresponding to each pole.

---

## 4. Representation by Summation of Exponentials

Using the inverse z-transform on each IIR term (conveninent z-transform pair, equivalent to the contour integral above):

$$
\mathcal{Z}^{-1}\{\frac{R_k}{1 - p_k z^{-1}}\} = R_k p_k^n u[n]
$$

where $u[n]$ is the unit step.  

Thus, **any channel with a rational polynomial transfer function can be represented as a summation of exponential terms of the poles**:

$$
h[n] = \sum_{k=1}^{N} R_k p_k^n u[n]
$$

---

## 5. Poles Determine Stability

A discrete-time system is **stable** if its impulse response $h[n]$ is absolutely summable:

$$
\sum_{n=0}^{\infty} |h[n]| < \infty
$$

From the exponential representation above, this requires:

$$
|p_k| < 1 \quad \forall k
$$

Hence, **the location of poles entirely determines the stability of the system**.

---

## 6. FIR Filters and Stability of Inverse

For an FIR filter:

$$
H_{\text{FIR}}(z) = b_0 + b_1 z^{-1} + \dots + b_M z^{-M}
$$

- It has **no poles** except at $z=0$ (for a proper causal FIR).  
- Its impulse response $h[n]$ is finite in length, so it is always **absolutely summable**, i.e., always stable.

### Stability of the Inverse Filter

The **inverse filter** is defined as:

$$
H_{\text{FIR}}^{-1}(z) = \frac{1}{H_{\text{FIR}}(z)}
$$

- The **poles of the inverse filter** are exactly the **zeros of the original FIR filter**.  
- If any zero of $H_{\text{FIR}}(z)$ lies **outside the unit circle** $(|z|>1)$, then the corresponding pole of $H_{\text{FIR}}^{-1}(z)$ is outside the unit circle.  
- This makes the **inverse filter unstable**, because its impulse response is no longer absolutely summable.

In other words:

- **Original FIR filter stability:** guaranteed by finite length of $h[n]$ (no poles outside unit circle).  
- **Inverse FIR filter stability:** depends on the location of zeros of the original filter. Zeros outside the unit circle → inverse is unstable.  

### Summary

| Filter Type | Poles | Stability Condition |
|------------|-------|-------------------|
| FIR        | Only at 0 | Always stable |
| IIR/FIR inverse | Zeros of original filter become poles | Stable if all original zeros inside unit circle |

Thus, **zeros of a channel or FIR filter determine the stability of its inverse**, while poles determine the stability of the filter itself.

---

## 7. Why Most Engineering Systems Have Rational Polynomial Transfer Functions

In engineering, **most physical systems are modeled by linear, time-invariant (LTI) differential or difference equations**.  

1. **Continuous-time systems:** governed by linear differential equations with constant coefficients:

$$
a_N \frac{d^N y(t)}{dt^N} + \dots + a_1 \frac{dy(t)}{dt} + a_0 y(t) = b_M \frac{d^M x(t)}{dt^M} + \dots + b_0 x(t)
$$

- Taking the Laplace transform, derivatives become multiplication by $s$, giving a **rational Laplace-domain transfer function**:

$$
H(s) = \frac{Y(s)}{X(s)} = \frac{b_0 + b_1 s + \dots + b_M s^M}{a_0 + a_1 s + \dots + a_N s^N}
$$

2. **Discrete-time systems:** governed by linear difference equations with constant coefficients:

$$
y[n] + a_1 y[n-1] + \dots + a_N y[n-N] = b_0 x[n] + b_1 x[n-1] + \dots + b_M x[n-M]
$$

- Taking the z-transform converts shifts into powers of $z^{-1}$, giving a **rational polynomial transfer function**:

$$
H(z) = \frac{Y(z)}{X(z)} = \frac{b_0 + b_1 z^{-1} + \dots + b_M z^{-M}}{1 + a_1 z^{-1} + \dots + a_N z^{-N}}
$$

**Conclusion:**  

- **Linearity + time invariance + finite memory** ⇒ transfer function is a **ratio of polynomials**, i.e., rational polynomial.  
- This is why **almost all engineered filters and communication channels are modeled as RP systems**, even if the physical system seems complex.

---
