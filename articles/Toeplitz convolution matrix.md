# Comparison: Full Convolution Matrix vs. Truncated (Causal) Toeplitz Matrix

## 1. Definitions

In linear algebra, a **Toeplitz matrix** or diagonal-constant matrix, named after Otto Toeplitz, is a matrix in which each descending diagonal from left to right is constant. For instance, the following matrix is a Toeplitz matrix:

$$
\left[\begin{array}{lllll}
a & b & c & d & e \\
f & a & b & c & d \\
g & f & a & b & c \\
h & g & f & a & b \\
i & h & g & f & a
\end{array}\right] .
$$

We can consider a **causal** channel impulse response (CIR) of length $L$:

$$
\mathbf{h} = [h_0, h_1, \dots, h_{L-1}]^T
$$

and an input sequence of length $N$:

$$
\mathbf{x} = [x_0, x_1, \dots, x_{N-1}]^T.
$$

However, either or both can be noncausal, and this is reflected in the structure of the Toeplitz matrix. This is discussed in section 3.  
Let's look at the convolution matrix definition below first.

## 2. Full Convolution Matrix

The **full convolution** output length is $N+L-1$.  
The full convolution matrix $\mathbf{H}_{\text{full}}$ has dimensions $(N+L-1) \times N$.

### Example with $N=4, L=3$

$$
\mathbf{H}_{\text{full}} =
\begin{bmatrix}
h_0 & 0 & 0 & 0 \\
h_1 & h_0 & 0 & 0 \\
h_2 & h_1 & h_0 & 0 \\
0 & h_2 & h_1 & h_0 \\
0 & 0 & h_2 & h_1 \\
0 & 0 & 0 & h_2
\end{bmatrix}
$$

Output:

$$
\mathbf{y}_{\text{full}} = \mathbf{H}_{\text{full}} \mathbf{x} =
\begin{bmatrix}
h_0 x_0 \\
h_1 x_0 + h_0 x_1 \\
h_2 x_0 + h_1 x_1 + h_0 x_2 \\
h_2 x_1 + h_1 x_2 + h_0 x_3 \\
h_2 x_2 + h_1 x_3 \\
h_2 x_3
\end{bmatrix}
$$

---

## 3. Example computation of the (full) Toeplitz convolution matrix
Notice that $y[i]$ is the dot product of $x$ and $i_{th}$ row of H.  

### 1. Setup

* Input $x[n]$ has **negative indices**:

$$
x = [x_{-1}, x_0, x_1]^T
$$

* Channel $h[k]$ also has **negative taps**:

$$
h = [h_{-1}, h_0, h_1]^T = [1, 2, 1]
$$

* Convolution formula:

$$
y[n] = \sum_k h[k] x[n-k]
$$

* We **do not assume zero before n=0**, because input is non-causal.

---

### 2. Compute outputs manually

Let’s compute a few outputs for clarity:

1. **$y_{-1}$:**

$$
y_{-1} = h[-1] x[-1 - (-1)] + h[0] x[-1 - 0] + h[1] x[-1 - 1]
= h[-1] x[0] + h[0] x[-1] + h[1] x[-2]
= 1·x_0 + 2·x_{-1} + 1·0
= 2 x_{-1} + x_0
$$

2. **$y_0$:**

$$
y_0 = h[-1] x[1] + h[0] x[0] + h[1] x[-1]
= 1·x_1 + 2·x_0 + 1·x_{-1}
= x_1 + 2 x_0 + x_{-1}
$$

3. **$y_1$:**

$$
y_1 = h[-1] x[2] + h[0] x[1] + h[1] x[0]
= 1·0 + 2·x_1 + 1·x_0
= x_0 + 2 x_1
$$

---

### 3. Toeplitz matrix

Columns = input samples $[x_{-1}, x_0, x_1]$  
Rows = output samples $[y_{-1}, y_0, y_1]$

$$
H =
\begin{bmatrix}
h[0] & h[-1] & 0 \\
h[1] & h[0] & h[-1] \\ 
0 & h[1] & h[0] \\         
\end{bmatrix}
$$

$$
\begin{bmatrix}
2 & 1 & 0 \\
1 & 2 & 1 \\
0 & 1 & 2 \\
\end{bmatrix}
$$

✅**Note**: This is not a lower triangular matrix. The convolution matrix can in general have any type of nonzero entry layout within Toeplitz strcuture.  

---


## 3. Truncated (Causal) Toeplitz Matrix

In the [paper](https://ieeexplore.ieee.org/document/10974467), the channel matrix $\hat{H}$ is an $N \times N$ **lower-triangular Toeplitz matrix** constructed by **padding** $\mathbf{h}$ with zeros to length $N$:

$$
\mathbf{h}_{\text{ext}} = [h_0, h_1, \dots, h_{L-1}, 0, \dots, 0]^T \quad (\text{length } N)
$$

$$
\hat{H} = \mathcal{T}(\mathbf{h}_{\text{ext}})
$$

### Same example ($N=4, L=3$):

$$
\hat{H} =
\begin{bmatrix}
h_0 & 0 & 0 & 0 \\
h_1 & h_0 & 0 & 0 \\
h_2 & h_1 & h_0 & 0 \\
0 & h_2 & h_1 & h_0
\end{bmatrix}
$$

Output (truncated convolution):

$$
\mathbf{y} = \hat{H} \mathbf{x} =
\begin{bmatrix}
h_0 x_0 \\
h_1 x_0 + h_0 x_1 \\ 
h_2 x_0 + h_1 x_1 + h_0 x_2 \\
h_2 x_1 + h_1 x_2 + h_0 x_3
\end{bmatrix}
$$

This is **exactly the first $N$ outputs** of the full convolution.

---

## 4. Information Loss

The truncated matrix **discards the last $L-1$ output samples** of the full convolution:

For $N=4, L=3$, the lost outputs are:

$$
y_4 = h_2 x_2 + h_1 x_3, \quad y_5 = h_2 x_3.
$$

Physically, these correspond to **inter-block interference** if consecutive data blocks are transmitted without guard intervals.

---

## Section 5: Causality Comparison with Concrete Example

Let me clarify and provide a concrete example that shows how **full Toeplitz matrices** can represent **non-causal channels** if we're not careful, while **truncated lower-triangular Toeplitz matrices** enforce causality.

### **Causal vs. Non-Causal Channel Example**

Consider a **non-causal channel** where the output depends on **future inputs**. This can happen in systems with intentional delay or in equalizer design.

**Channel impulse response** (non-causal, length \(L=3\)):

$$
\mathbf{h}_{\text{nc}} = [h_{-1}, h_0, h_1]^T
$$

Where:
- $h_{-1}$ = weight for **future** input $x[n+1]$
- $h_0$ = weight for current input $x[n]$
- $h_1$ = weight for past input $x[n-1]$

System equation:

$$
y[n] = h_{-1} x[n+1] + h_0 x[n] + h_1 x[n-1]
$$

### **1. Full Convolution Matrix Representation**

For input $\mathbf{x} = [x_0, x_1, x_2, x_3]^T$ ($N=4$), the **full convolution matrix** would be:

$$
\mathbf{H}_{\text{full,nc}} =
\begin{bmatrix}
h_0 & h_{-1} & 0 & 0 \\
h_1 & h_0 & h_{-1} & 0 \\
0 & h_1 & h_0 & h_{-1} \\
0 & 0 & h_1 & h_0 \\
0 & 0 & 0 & h_1
\end{bmatrix}
$$

**Note**: This matrix has **non-zero elements above the main diagonal** (circled below for clarity):

$$
\mathbf{H}_{\text{full,nc}} =
\begin{bmatrix}
h_0 & \color{red}{h_{-1}} & 0 & 0 \\
h_1 & h_0 & \color{red}{h_{-1}} & 0 \\
0 & h_1 & h_0 & \color{red}{h_{-1}} \\
0 & 0 & h_1 & h_0 \\
0 & 0 & 0 & h_1
\end{bmatrix}
$$

#### **Output calculation**:

$$
\mathbf{y}_{\text{full,nc}} = \mathbf{H}_{\text{full,nc}} \mathbf{x} =
\begin{bmatrix}
h_0 x_0 + h_{-1} x_1 \\
h_1 x_0 + h_0 x_1 + h_{-1} x_2 \\
h_1 x_1 + h_0 x_2 + h_{-1} x_3 \\
h_1 x_2 + h_0 x_3 \\
h_1 x_3
\end{bmatrix}
$$

#### **Why this represents a non-causal system**:

Look at the **first output sample** $y_0$:

$$
y_0 = h_0 x_0 + h_{-1} x_1
$$

This depends on **$x_1$** which is a **future input** relative to output time $0$.

Similarly:
- $y_1$ depends on $x_2$ (future)
- $y_2$ depends on $x_3$ (future)

### **2. Truncated Lower-Triangular Toeplitz Representation**

Now, if we try to create a **truncated lower-triangular Toeplitz matrix** from this same non-causal channel 

$$
\mathbf{h}_{nc} = [h_{-1}, h_0, h_1]^T
$$, 

we must **reorder it to be causal**.

To make it causal, we need an impulse response that starts at time 0 or later. Let's **shift the non-causal response** to make it causal:

**Causal version** (shifted by 1 sample):

$$
\mathbf{h}_{\text{causal}} = [h_0, h_1, 0]^T
$$

But this **loses** the \(h_{-1}\) component entirely!

### **Truncated matrix for causal system**:

Using only the causal part:

$$
\hat{H}_{\text{causal}} =
\begin{bmatrix}
h_0 & 0 & 0 & 0 \\
h_1 & h_0 & 0 & 0 \\
0 & h_1 & h_0 & 0 \\
0 & 0 & h_1 & h_0
\end{bmatrix}
$$

**Output**:

$$
\mathbf{y}_{\text{causal}} = \hat{H}_{\text{causal}} \mathbf{x} =
\begin{bmatrix}
h_0 x_0 \\
h_1 x_0 + h_0 x_1 \\
h_1 x_1 + h_0 x_2 \\
h_1 x_2 + h_0 x_3
\end{bmatrix}
$$


## 6. Why Use the Truncated Model?

1. **Invertibility**: $\hat{H}$ is $N \times N$ and can be inverted for ZF equalizer design.
2. **Block-based processing**: Matches OFDM symbol detection where each block of $N$ samples is processed independently (after CP removal).
3. **Simplified analysis**: PCA, eigenvalue decomposition, and RNN weight configuration require a fixed $N \times N$ matrix structure.
4. **Negligible loss when $N \gg L$**: The lost $L-1$ samples are a small fraction of total outputs.

---

## 7. Summary in Table Form

| Aspect | Full Convolution Matrix | Truncated Toeplitz Matrix |
|--------|------------------------|---------------------------|
| **Size** | $(N+L-1) \times N$ | $N \times N$ |
| **Output length** | $N+L-1$ | $N$ |
| **Causality** | Physically causal, but not square | Causal + square |
| **Information loss** | None | Last $L-1$ output samples |
| **Use in equalizer design** | Cannot be directly inverted | Invertible (enables ZF equalizer $\hat{H}^{-1}$) |
| **Suitability for PCA** | Difficult (non-square) | Straightforward (square covariance structure) |

---

## 8. Key Takeaway

The **truncated lower-triangular Toeplitz matrix** is a **causal, square approximation** of the true convolution, obtained by keeping only the first $N$ output samples. It enables tractable matrix analysis and equalizer design while preserving causality, at the cost of discarding a small tail of the convolution output when $N \gg L$.
