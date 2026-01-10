# Log-Likelihood Trick Proof

## 1. Setup

Suppose we have a probability distribution $p_\theta(x)$ depending on parameters $\theta$, and we want to compute the gradient of an expectation:

$$
\nabla_\theta \mathbb{E}_{x\sim p_\theta}[f(x)] = \nabla_\theta \int f(x) p_\theta(x) dx
$$

for some function $f(x)$ (e.g., reward in RL).

---

## 2. Move gradient inside the integral

If $p_\theta(x)$ is differentiable w.r.t. $\theta$, we can write:

$$
\nabla_\theta \int f(x) p_\theta(x) dx = \int f(x) \nabla_\theta p_\theta(x) dx
$$

---

## 3. Multiply and divide by $p_\theta(x)$

Assuming $p_\theta(x) > 0$ for all $x$, we can write:

$$
\nabla_\theta p_\theta(x) = p_\theta(x) \frac{\nabla_\theta p_\theta(x)}{p_\theta(x)} = p_\theta(x) \nabla_\theta \log p_\theta(x)
$$

This is the key step.

---

## 4. Substitute back

Now the gradient becomes:

$$
\int f(x) \nabla_\theta p_\theta(x) dx = \int f(x) p_\theta(x) \nabla_\theta \log p_\theta(x) dx
$$

---

## 5. Recognize expectation form

By the definition of expectation w.r.t. $p_\theta(x)$:

$$
\int f(x) p_\theta(x) \nabla_\theta \log p_\theta(x) dx = \mathbb{E}_{x\sim p_\theta}[f(x) \nabla_\theta \log p_\theta(x)]
$$

This is exactly the **log-likelihood trick / score function trick**:

$$
\boxed{\nabla_\theta \mathbb{E}_{x\sim p_\theta}[f(x)] = \mathbb{E}_{x\sim p_\theta}[f(x) \nabla_\theta \log p_\theta(x)]}
$$

---

## 6. Intuition

* The gradient of the expectation is **the expectation of the gradient of the log-probability multiplied by the function value**.
* It allows us to **estimate the gradient using only samples** $x\sim p_\theta$, which is crucial in RL where $f(x)$ (reward) is a black-box.
