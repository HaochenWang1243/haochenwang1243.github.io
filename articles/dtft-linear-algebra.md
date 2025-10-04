
## **Understanding the DTFT: A Linear Algebra Perspective**

The Discrete-Time Fourier Transform (DTFT) is more than just a formula; it's a profound geometric statement. It reveals that any discrete signal can be represented as a sum of infinitely many, continuously-indexed oscillating components. This article will trace this elegant concept, starting from the dot product and culminating in the DTFT. 

I came up with the mathematical derivations in this article, and AI suggested phrasing and organization. Neither of us is flawless, so take these derivations with a grain of salt (or a whole shaker 😄).

### **1. The Foundation: Inner Products in Complex Space**

Our journey begins with the generalization of the dot product to complex vectors. In $\mathbb{C}^m$, the inner product for vectors $\vec{u}$ and $\vec{v}$ is defined as:

$$
\langle \vec{u}, \vec{v} \rangle = \sum_{n} \vec{u}[n] \overline{\vec{v}[n]}
$$

This operation has two key properties:
* **Linearity in the first argument:** $\langle a\vec{u}_1 + b\vec{u}_2, \vec{v} \rangle = a\langle \vec{u}_1, \vec{v} \rangle + b\langle \vec{u}_2, \vec{v} \rangle$
* **Conjugate linearity in the second argument:** $\langle \vec{v}, a\vec{u}_1 + b\vec{u}_2 \rangle = \overline{a}\langle \vec{v}, \vec{u}_1 \rangle + \overline{b}\langle \vec{v}, \vec{u}_2 \rangle$

### **2. Decomposing Vectors onto an Orthonormal Basis**

In a finite-dimensional space with an orthonormal basis $\{\vec{u}_1, \ldots, \vec{u}_n\}$, any vector $\vec{v}$ can be expressed as a linear combination of the basis vectors:

$$
\vec{v} = \sum_{i=1}^n C_i \vec{u}_i
$$

A key result is that the coefficients $C_j$ (the "weights") are effortlessly found by projecting the vector onto each basis element:

$$
\langle \vec{v}, \vec{u}_j \rangle = \langle \sum_{i=1}^n C_i \vec{u}_i, \vec{u}_j \rangle = \sum_{i=1}^n C_i \langle \vec{u}_i, \vec{u}_j \rangle = C_j
$$

The orthonormality condition $\langle \vec{u}_i, \vec{u}_j \rangle = \delta[i-j]$ causes all terms in the sum to vanish except for $i=j$.

### **3. The Conceptual Leap: A Continuous Basis**

Now, we make a critical leap. What if the orthonormal basis is not finite but **uncountably infinite**, with its elements indexed by a continuous variable $x \in [-\pi, \pi]$? The sum becomes an integral:

$$
\vec{v} = \int_{-\pi}^{\pi} C(x) \vec{u}(x) \, dx
$$

The central question is: does the rule $C(x_0) = \langle \vec{v}, \vec{u}(x_0) \rangle$ still hold? Let's verify:

$$
\langle \vec{v}, \vec{u}(x_0) \rangle = \left\langle \int_{-\pi}^{\pi} C(x) \vec{u}(x) dx, \vec{u}(x_0) \right\rangle = \int_{-\pi}^{\pi} C(x) \langle \vec{u}(x), \vec{u}(x_0) \rangle dx
$$

For this to yield $C(x_0)$, the inner product $\langle \vec{u}(x), \vec{u}(x_0) \rangle$ must behave like a Dirac delta function $\delta(x - x_0)$, not the discrete Kronecker delta.

$$
\int_{-\pi}^{\pi} C(x) \langle \vec{u}(x), \vec{u}(x_0) \rangle \, dx = \int_{-\pi}^{\pi} C(x) \delta(x - x_0) \, dx = C(x_0)
$$

### **4. The "Magic" Basis: Complex Exponentials and the Delta Function**

So, what set of vectors $\vec{u}(x)$ satisfies $\langle \vec{u}(x), \vec{u}(x_0) \rangle = \delta(x - x_0)$?

The answer is the set of **complex exponentials**. For $x = \omega \in (-\pi, \pi)$, we define:

$$
\vec{u}(\omega) = [ \ldots, e^{-j\omega 1}, e^{-j\omega 0}, e^{j\omega 1}, e^{j\omega 2}, \ldots ]^T \quad \text{for } n \in \mathbb{Z}
$$

Let's prove the orthogonality condition holds:

$$
\langle \vec{u}(\omega), \vec{u}(\omega_0) \rangle = \sum_{n=-\infty}^{\infty} e^{j\omega n} e^{-j\omega_0 n} = \sum_{n=-\infty}^{\infty} e^{j(\omega - \omega_0)n}
$$

This infinite sum is zero for $\omega \neq \omega_0$ due to destructive interference of the rotating phasors. However, when $\omega = \omega_0$, every term is 1, and the sum diverges to infinity. This is the defining behavior of the Dirac delta function. Since $\omega, \omega_0 \in (-\pi, \pi)$, the difference $\omega - \omega_0 \in (-2\pi, 2\pi)$, and the only point where they are equal is at zero. In fact:

$$
\sum_{n=-\infty}^{\infty} e^{j(\omega - \omega_0)n} = 2\pi \delta(\omega - \omega_0) \quad \text{over } \omega \in (-\pi, \pi)
$$

Where does the $2\pi$ come from? This is an interesting question on its own. I include the intuition but omit the rigor here for now, but I may write about it in the future!

### **5. The Grand Finale: The DTFT as an Orthogonal Expansion**

This brings us directly to the DTFT. Consider a discrete-time signal $\vec{x} = x[n]$ as our vector.

* **Analysis (The Forward DTFT):** The DTFT finds the continuous set of weights $C(\omega)$ for our basis $\vec{u}(\omega)$.

$$
X(e^{j\omega}) = \sum_{n=-\infty}^{\infty} x[n] e^{-j\omega n} = \langle \vec{x}, \vec{u}(\omega) \rangle
$$

Here, $X(e^{j\omega})$ is the weight $C(\omega)$, the projection of the signal onto the basis vector of frequency $\omega$.

* **Synthesis (The Inverse DTFT):** The original signal is reconstructed by summing (integrating) all the basis vectors, each scaled by its weight.

$$
x[n] = \frac{1}{2\pi} \int_{-\pi}^{\pi} X(e^{j\omega}) e^{j\omega n} \ d\omega = \frac{1}{2\pi} \int_{-\pi}^{\pi} C(\omega) \vec{u}(\omega)[n] \ d\omega
$$

The factor $\frac{1}{2\pi}$ is a normalization constant to ensure perfect reconstruction given our definition of the inner product.

## **Summary**

In conclusion, the DTFT reveals a powerful orthogonal decomposition. The set

$$
\{ \vec{u}(\omega) \in \mathbb{C}^{\infty} \mid \vec{u}(\omega) = [e^{j\omega n} \text{ for } n \in \mathbb{Z}], \omega \in (-\pi, \pi) \}
$$

forms a continuous orthonormal basis for the space of absolute-summable sequences. The DTFT $X(e^{j\omega})$ provides the weights of this basis, offering a complete and elegant description of a signal in the frequency domain. 

The DTFT is not just a theoretical construct; it underpins many practical aspects of digital communications. For example, it directly informs the **Nyquist sampling theorem**, which states that a bandlimited signal with maximum frequency $f_\text{max}$ can be perfectly reconstructed from samples taken at a rate $f_s \ge 2 f_\text{max}$. This principle ensures that digital communication systems can sample, transmit, and reconstruct signals without aliasing. Moreover, the DTFT forms the foundation for **filter design**, **modulation schemes**, and **spectrum analysis**, making it an indispensable tool for engineers designing modern communication systems.
