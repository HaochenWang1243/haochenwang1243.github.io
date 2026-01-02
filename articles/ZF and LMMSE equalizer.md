# Equalizer Derivation and Comparison: ZF vs LMMSE

## System Model

We consider a frequency-selective channel with convolution:

$$
y[n] = \sum_{k=0}^{L-1} h[k]x[n-k] + w[n]
$$

where:
- $x[n]$: Transmitted symbols
- $h[k]$: Channel impulse response (length $L$)
- $w[n]$: Additive white Gaussian noise ∼ $\mathcal{CN}(0, \sigma_w^2)$
- $y[n]$: Received symbols

## Convolution Matrix Representation

For a block of $N$ received symbols with $M$ transmitted symbols, we form the **convolution matrix**:

$$
\mathbf{y} = \mathbf{Hx} + \mathbf{w}
$$

where:

$$
\mathbf{H} = 
\begin{bmatrix}
h[0] & 0 & 0 & \cdots & 0 \\
h[1] & h[0] & 0 & \cdots & 0 \\
h[2] & h[1] & h[0] & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
h[L-1] & h[L-2] & h[L-3] & \cdots & 0 \\
0 & h[L-1] & h[L-2] & \cdots & h[0] \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \cdots & h[L-1]
\end{bmatrix}
$$

Dimensions:
- $\mathbf{H}$: $N \times M$ (typically $N = M + L - 1$)
- $\mathbf{x}$: $M \times 1$
- $\mathbf{y}$: $N \times 1$
- $\mathbf{w}$: $N \times 1$

## 1. Zero-Forcing (ZF) Equalizer Derivation

### Objective
Find linear equalizer $\mathbf{W}_{\text{ZF}}$ that completely eliminates Inter-Symbol Interference (ISI):

$$
\hat{\mathbf{x}}_{\text{ZF}} = \mathbf{W}_{\text{ZF}}\mathbf{y} \approx \mathbf{x}
$$

### Assumptions
1. Perfect channel knowledge ($\mathbf{H}$ known)
2. Noise is ignored in the optimization

### Derivation
We want the equalizer to invert the channel:

$$
\mathbf{W}_{\text{ZF}}\mathbf{H} = \mathbf{I}_M
$$

where $\mathbf{I}_M$ is the $M \times M$ identity matrix.

#### Case 1: Square Matrix ($N = M$)
If $\mathbf{H}$ is square and invertible:

$$
\mathbf{W}_{\text{ZF}} = \mathbf{H}^{-1}
$$

#### Case 2: Tall Matrix ($N > M$) - Overdetermined
More common in practice. We use the **left pseudoinverse**:

Multiply both sides of $\mathbf{W}_{\text{ZF}}\mathbf{H} = \mathbf{I}$ by $\mathbf{H}^H$:

$$
\mathbf{W}_{\text{ZF}}\mathbf{H}\mathbf{H}^H = \mathbf{H}^H
$$

Assuming $\mathbf{H}\mathbf{H}^H$ is invertible:

$$
\mathbf{W}_{\text{ZF}} = \mathbf{H}^H(\mathbf{H}\mathbf{H}^H)^{-1}
$$

Alternatively, using the **Moore-Penrose pseudoinverse**:

$$
\mathbf{W}_{\text{ZF}} = (\mathbf{H}^H\mathbf{H})^{-1}\mathbf{H}^H
$$

These are equivalent when $\mathbf{H}$ has full column rank.

### Least Squares Interpretation
ZF equalizer minimizes the residual error **ignoring noise**:

$$
\mathbf{W}_{\text{ZF}} = \arg\min_{\mathbf{W}} \|\mathbf{W}\mathbf{y} - \mathbf{x}\|^2
$$

Solution to normal equations:

$$
\mathbf{H}^H\mathbf{H}\hat{\mathbf{x}} = \mathbf{H}^H\mathbf{y}
$$

$$
\hat{\mathbf{x}}_{\text{ZF}} = (\mathbf{H}^H\mathbf{H})^{-1}\mathbf{H}^H\mathbf{y}
$$

### Final ZF Equalizer

$$
\boxed{\mathbf{W}_{\text{ZF}} = (\mathbf{H}^H\mathbf{H})^{-1}\mathbf{H}^H}
$$

## 2. Linear Minimum Mean Square Error (LMMSE) Equalizer Derivation

### Objective
Find linear equalizer $\mathbf{W}_{\text{LMMSE}}$ that minimizes the **mean square error**:

$$
\mathbf{W}_{\text{LMMSE}} = \arg\min_{\mathbf{W}} \mathbb{E}\left[ \|\mathbf{W}\mathbf{y} - \mathbf{x}\|^2 \right]
$$

### Assumptions
1. Perfect channel knowledge ($\mathbf{H}$ known)
2. Known noise statistics: $\mathbb{E}[\mathbf{w}\mathbf{w}^H] = \sigma_w^2\mathbf{I}$
3. Known signal statistics: $\mathbb{E}[\mathbf{x}\mathbf{x}^H] = \sigma_x^2\mathbf{I}$
4. Signal and noise uncorrelated: $\mathbb{E}[\mathbf{x}\mathbf{w}^H] = \mathbf{0}$

### Derivation
The cost function is:

$$
J(\mathbf{W}) = \mathbb{E}\left[ \|\mathbf{W}\mathbf{y} - \mathbf{x}\|^2 \right]
$$

#### Step 1: Expand the expectation

$$
J(\mathbf{W}) = \mathbb{E}\left[ \text{tr}\left( (\mathbf{W}\mathbf{y} - \mathbf{x})(\mathbf{W}\mathbf{y} - \mathbf{x})^H \right) \right]
$$

$$
= \text{tr}\left( \mathbf{W}\mathbb{E}[\mathbf{y}\mathbf{y}^H]\mathbf{W}^H - \mathbf{W}\mathbb{E}[\mathbf{y}\mathbf{x}^H] - \mathbb{E}[\mathbf{x}\mathbf{y}^H]\mathbf{W}^H + \mathbb{E}[\mathbf{x}\mathbf{x}^H] \right)
$$

#### Step 2: Compute required expectations
$$
\mathbf{R}_{yy} = \mathbb{E}[\mathbf{y}\mathbf{y}^H] = \mathbb{E}[(\mathbf{Hx} + \mathbf{w})(\mathbf{Hx} + \mathbf{w})^H]
$$

$$
= \mathbf{H}\mathbb{E}[\mathbf{x}\mathbf{x}^H]\mathbf{H}^H + \mathbb{E}[\mathbf{w}\mathbf{w}^H] + \cancel{\mathbf{H}\mathbb{E}[\mathbf{x}\mathbf{w}^H]} + \cancel{\mathbb{E}[\mathbf{w}\mathbf{x}^H]\mathbf{H}^H}
$$

$$
= \sigma_x^2 \mathbf{H}\mathbf{H}^H + \sigma_w^2 \mathbf{I}
$$

$$
\mathbf{R}_{xy} = \mathbb{E}[\mathbf{x}\mathbf{y}^H] = \mathbb{E}[\mathbf{x}(\mathbf{Hx} + \mathbf{w})^H]
$$

$$
= \mathbb{E}[\mathbf{x}\mathbf{x}^H]\mathbf{H}^H + \cancel{\mathbb{E}[\mathbf{x}\mathbf{w}^H]}
$$

$$
= \sigma_x^2 \mathbf{H}^H
$$

Similarly: $\mathbf{R}_{yx} = \mathbf{R}_{xy}^H = \sigma_x^2 \mathbf{H}$

#### Step 3: Take gradient and set to zero
$$
\nabla_{\mathbf{W}} J(\mathbf{W}) = 2\mathbf{W}\mathbf{R}_{yy} - 2\mathbf{R}_{xy} = \mathbf{0}
$$

$$
\mathbf{W}\mathbf{R}_{yy} = \mathbf{R}_{xy}
$$

#### Step 4: Solve for $\mathbf{W}$
$$
\mathbf{W}_{\text{LMMSE}} = \mathbf{R}_{xy} \mathbf{R}_{yy}^{-1}
$$

$$
= \sigma_x^2 \mathbf{H}^H \left( \sigma_x^2 \mathbf{H}\mathbf{H}^H + \sigma_w^2 \mathbf{I} \right)^{-1}
$$

### Final LMMSE Equalizer

$$
\boxed{\mathbf{W}_{\text{LMMSE}} = \mathbf{H}^H \left( \mathbf{H}\mathbf{H}^H + \frac{\sigma_w^2}{\sigma_x^2} \mathbf{I} \right)^{-1}}
$$

Alternative form (using matrix inversion lemma):

$$
\boxed{\mathbf{W}_{\text{LMMSE}} = \left( \mathbf{H}^H\mathbf{H} + \frac{\sigma_w^2}{\sigma_x^2} \mathbf{I} \right)^{-1} \mathbf{H}^H}
$$

## 3. Comparison of ZF and LMMSE

### Mathematical Relationship
$$
\mathbf{W}_{\text{LMMSE}} = \left( \mathbf{H}^H\mathbf{H} + \frac{\sigma_w^2}{\sigma_x^2} \mathbf{I} \right)^{-1} \mathbf{H}^H
$$

$$
\mathbf{W}_{\text{ZF}} = \left( \mathbf{H}^H\mathbf{H} \right)^{-1} \mathbf{H}^H
$$

LMMSE adds a **regularization term** $\frac{\sigma_w^2}{\sigma_x^2} \mathbf{I}$.

### Key Properties Comparison

| Property | Zero-Forcing (ZF) | LMMSE |
|----------|-------------------|--------|
| **Objective** | Eliminate ISI completely | Minimize MSE |
| **Noise Handling** | Ignores noise, amplifies it | Explicitly accounts for noise |
| **SNR Regime** | Optimal at high SNR ($\text{SNR} \to \infty$) | Optimal at all SNRs (among linear) |
| **Complexity** | Lower (no SNR term) | Slightly higher (regularization) |
| **Robustness** | Poor for ill-conditioned $\mathbf{H}$ | More robust (Tikhonov regularization) |
| **Inversion Stability** | Problematic if $\mathbf{H}^H\mathbf{H}$ near-singular | Regularized inversion more stable |
| **Bias** | Unbiased estimator | Biased estimator (reduces variance) |

### Performance Metrics

**MSE for ZF:**

$$
\text{MSE}_{\text{ZF}} = \sigma_w^2 \cdot \text{tr}\left( (\mathbf{H}^H\mathbf{H})^{-1} \right)
$$

**MSE for LMMSE:**

$$
\text{MSE}_{\text{LMMSE}} = \text{tr}\left( \left( \mathbf{I} + \frac{\sigma_x^2}{\sigma_w^2} \mathbf{H}^H\mathbf{H} \right)^{-1} \right) \cdot \sigma_x^2
$$

### Special Cases

1. **High SNR ($\sigma_w^2 \to 0$):**
   
$$
\mathbf{W}_{\text{LMMSE}} \to (\mathbf{H}^H\mathbf{H})^{-1}\mathbf{H}^H = \mathbf{W}_{\text{ZF}}
$$

   LMMSE approaches ZF performance

2. **Low SNR ($\sigma_w^2 \to \infty$):**

$$
\mathbf{W}_{\text{LMMSE}} \to \mathbf{0}
$$

   Better to ignore noisy measurements

3. **White Channel ($\mathbf{H}^H\mathbf{H} = \mathbf{I}$):**
   Both equalizers are identical:

$$
\mathbf{W}_{\text{ZF}} = \mathbf{W}_{\text{LMMSE}} = \mathbf{H}^H
$$

## 4. Numerical Example

### System Parameters
- Channel: $h = [0.8, 0.5, 0.3]$ (length $L = 3$)
- Transmitted symbols: $\mathbf{x} = [1, -1, 0.5, -0.5]^T$ (length $M = 4$)
- Noise variance: $\sigma_w^2 = 0.1$
- Signal power: $\sigma_x^2 = 1$
- SNR = $10\log_{10}(\sigma_x^2/\sigma_w^2) = 10$ dB

### Step 1: Construct Convolution Matrix
For $M = 4$, $L = 3$, we get $N = M + L - 1 = 6$ received samples:

$$
\mathbf{H} = 
\begin{bmatrix}
0.8 & 0 & 0 & 0 \\
0.5 & 0.8 & 0 & 0 \\
0.3 & 0.5 & 0.8 & 0 \\
0 & 0.3 & 0.5 & 0.8 \\
0 & 0 & 0.3 & 0.5 \\
0 & 0 & 0 & 0.3
\end{bmatrix}_{6 \times 4}
$$

### Step 2: Generate Received Signal
Transmitted signal: $\mathbf{x} = [1, -1, 0.5, -0.5]^T$

Noise (example realization): 

$$
\mathbf{w} = [0.1, -0.05, 0.15, -0.1, 0.05, -0.15]^T
$$

Received signal:

$$
\mathbf{y} = \mathbf{Hx} + \mathbf{w}
$$

Calculate $\mathbf{Hx}$:

$$
\mathbf{Hx} = 
\begin{bmatrix}
0.8\times1 = 0.8 \\
0.5\times1 + 0.8\times(-1) = -0.3 \\
0.3\times1 + 0.5\times(-1) + 0.8\times0.5 = 0.2 \\
0.3\times(-1) + 0.5\times0.5 + 0.8\times(-0.5) = -0.45 \\
0.3\times0.5 + 0.5\times(-0.5) = -0.1 \\
0.3\times(-0.5) = -0.15
\end{bmatrix}
$$

Add noise:

$$
\mathbf{y} = [0.9, -0.35, 0.35, -0.55, -0.05, -0.3]^T
$$

### Step 3: Compute ZF Equalizer

$$
\mathbf{H}^H\mathbf{H} = 
\begin{bmatrix}
0.98 & 0.40 & 0.24 & 0 \\
0.40 & 0.98 & 0.40 & 0.24 \\
0.24 & 0.40 & 0.98 & 0.40 \\
0 & 0.24 & 0.40 & 0.98
\end{bmatrix}
$$

Inverse:

$$
(\mathbf{H}^H\mathbf{H})^{-1} = 
\begin{bmatrix}
1.37 & -0.68 & 0.17 & 0.17 \\
-0.68 & 1.85 & -0.85 & 0.17 \\
0.17 & -0.85 & 1.85 & -0.68 \\
0.17 & 0.17 & -0.68 & 1.37
\end{bmatrix}
$$

ZF equalizer:

$$
\mathbf{W}_{\text{ZF}} = (\mathbf{H}^H\mathbf{H})^{-1}\mathbf{H}^H
$$

Calculate $\mathbf{H}^H\mathbf{y}$:

$$
\mathbf{H}^H\mathbf{y} = 
\begin{bmatrix}
0.8\times0.9 + 0.5\times(-0.35) + 0.3\times0.35 = 0.62 \\
0.8\times(-0.35) + 0.5\times0.35 + 0.3\times(-0.55) = -0.44 \\
0.8\times0.35 + 0.5\times(-0.55) + 0.3\times(-0.05) = -0.01 \\
0.8\times(-0.55) + 0.5\times(-0.05) + 0.3\times(-0.3) = -0.615
\end{bmatrix}
$$

ZF estimate:

$$
\hat{\mathbf{x}}_{\text{ZF}} = \mathbf{W}_{\text{ZF}}\mathbf{y} = (\mathbf{H}^H\mathbf{H})^{-1}(\mathbf{H}^H\mathbf{y})
$$

$$
= 
\begin{bmatrix}
1.37\times0.62 + (-0.68)\times(-0.44) + 0.17\times(-0.01) + 0.17\times(-0.615) = 1.12 \\
-0.68\times0.62 + 1.85\times(-0.44) + (-0.85)\times(-0.01) + 0.17\times(-0.615) = -1.23 \\
0.17\times0.62 + (-0.85)\times(-0.44) + 1.85\times(-0.01) + (-0.68)\times(-0.615) = 0.68 \\
0.17\times0.62 + 0.17\times(-0.44) + (-0.68)\times(-0.01) + 1.37\times(-0.615) = -0.73
\end{bmatrix}
$$

### Step 4: Compute LMMSE Equalizer
Regularization term: $\frac{\sigma_w^2}{\sigma_x^2} = 0.1$

$$
\mathbf{H}^H\mathbf{H} + 0.1\mathbf{I} = 
\begin{bmatrix}
1.08 & 0.40 & 0.24 & 0 \\
0.40 & 1.08 & 0.40 & 0.24 \\
0.24 & 0.40 & 1.08 & 0.40 \\
0 & 0.24 & 0.40 & 1.08
\end{bmatrix}
$$

Inverse:

$$
(\mathbf{H}^H\mathbf{H} + 0.1\mathbf{I})^{-1} = 
\begin{bmatrix}
1.10 & -0.48 & 0.13 & 0.04 \\
-0.48 & 1.22 & -0.51 & 0.13 \\
0.13 & -0.51 & 1.22 & -0.48 \\
0.04 & 0.13 & -0.48 & 1.10
\end{bmatrix}
$$

LMMSE estimate:

$$
\hat{\mathbf{x}}_{\text{LMMSE}} = (\mathbf{H}^H\mathbf{H} + 0.1\mathbf{I})^{-1}(\mathbf{H}^H\mathbf{y}) = 
\begin{bmatrix}
1.10\times0.62 + (-0.48)\times(-0.44) + 0.13\times(-0.01) + 0.04\times(-0.615) = 0.96 \\
-0.48\times0.62 + 1.22\times(-0.44) + (-0.51)\times(-0.01) + 0.13\times(-0.615) = -1.02 \\
0.13\times0.62 + (-0.51)\times(-0.44) + 1.22\times(-0.01) + (-0.48)\times(-0.615) = 0.55 \\
0.04\times0.62 + 0.13\times(-0.44) + (-0.48)\times(-0.01) + 1.10\times(-0.615) = -0.65
\end{bmatrix}
$$

### Step 5: Performance Comparison

| Metric | ZF Equalizer | LMMSE Equalizer | True $\mathbf{x}$ |
|--------|--------------|-----------------|-------------------|
| Estimate 1 | 1.12 | 0.96 | 1.00 |
| Estimate 2 | -1.23 | -1.02 | -1.00 |
| Estimate 3 | 0.68 | 0.55 | 0.50 |
| Estimate 4 | -0.73 | -0.65 | -0.50 |

**MSE Calculation:**

ZF MSE:

$$
\text{MSE}_{\text{ZF}} = \frac{1}{4}\left[(1.12-1)^2 + (-1.23+1)^2 + (0.68-0.5)^2 + (-0.73+0.5)^2\right] = 0.032
$$

LMMSE MSE:

$$
\text{MSE}_{\text{LMMSE}} = \frac{1}{4}\left[(0.96-1)^2 + (-1.02+1)^2 + (0.55-0.5)^2 + (-0.65+0.5)^2\right] = 0.008
$$

**Noise Enhancement Factor (ZF):**

$$
\text{NEF}_{\text{ZF}} = \text{tr}((\mathbf{H}^H\mathbf{H})^{-1}) = 1.37 + 1.85 + 1.85 + 1.37 = 6.44
$$

Actual noise power amplified by 6.44×!

**Observations:**
1. ZF has larger deviations from true symbols due to noise amplification
2. LMMSE provides more accurate estimates with lower MSE
3. ZF estimates show over-shooting (typical of noise enhancement)
4. LMMSE provides a better bias-variance tradeoff
