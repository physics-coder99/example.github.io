# 📘 Stochastic Dynamics, Generators, and BAOAB — Complete Notes

These notes summarize the concepts of **Wiener processes, Itô calculus, SDE generators,
Langevin dynamics, and the BAOAB algorithm**, with full derivations and intuition.

---

## 1. Wiener Process (Brownian Motion)

A **Wiener process** \( W_t \) satisfies:

1. \( W_0 = 0 \)
2. Independent increments
3. \( W_t - W_s \sim \mathcal N(0, t-s) \)
4. Continuous but nowhere differentiable paths

Key scaling:
\[
dW_t \sim \sqrt{dt}, \qquad (dW_t)^2 = dt
\]

This identity is the foundation of **Itô calculus**.

---

## 2. Stochastic Differential Equations (SDEs)

General Itô SDE:
\[
dX_t = b(X_t)\,dt + \sigma(X_t)\,dW_t
\]

- \( b \): drift (deterministic)
- \( \sigma \): diffusion (random)

SDEs describe systems driven by noise.

---

## 3. Itô Calculus

### Itô multiplication rules
\[
dt^2 = 0, \quad dt\,dW_t = 0, \quad (dW_t)^2 = dt
\]

### Itô’s Lemma
For \( X_t \) solving an SDE and smooth \( f(t,x) \):

\[
df =
\left(\partial_t f + b\cdot\nabla f + \tfrac12(\sigma\sigma^\top):\nabla^2 f\right)dt
+ \nabla f\,\sigma\,dW_t
\]

The extra second-derivative term is the hallmark of stochastic calculus.

---

## 4. Langevin Dynamics

### Full (underdamped) Langevin SDE
\[
\begin{aligned}
dq_t &= \frac{p_t}{m}\,dt \\
dp_t &= -\nabla U(q_t)\,dt - \gamma p_t\,dt
+ \sqrt{2\gamma m\beta^{-1}}\,dW_t
\end{aligned}
\]

Invariant distribution:
\[
\pi(q,p) \propto e^{-\beta\left(\frac{p^2}{2m}+U(q)\right)}
\]

---

## 5. SDE Generator

### Definition
For
\[
dX_t = b(X_t)\,dt + \sigma(X_t)\,dW_t
\]

the **generator** is
\[
\boxed{
\mathcal L f
=
b\cdot\nabla f
+
\tfrac12(\sigma\sigma^\top):\nabla^2 f
}
\]

### Meaning
\[
\frac{d}{dt}\mathbb E[f(X_t)]
=
\mathbb E[(\mathcal L f)(X_t)]
\]

The generator captures infinitesimal evolution of expectations.

---

## 6. Langevin Generator

For \( x=(q,p) \):

\[
\boxed{
\mathcal L
=
\frac{p}{m}\cdot\nabla_q
-
\nabla U(q)\cdot\nabla_p
-
\gamma p\cdot\nabla_p
+
\gamma m\beta^{-1}\Delta_p
}
\]

### Why the Laplacian appears
- Noise acts only on momentum
- \( (dW)^2 = dt \)
- Second derivatives appear only in noisy variables

---

## 7. Generator Splitting (BAOAB)

We split:
\[
\mathcal L = \mathcal L_A + \mathcal L_B + \mathcal L_O
\]

### A — Drift
\[
\mathcal L_A = \frac{p}{m}\cdot\nabla_q
\]

### B — Force
\[
\mathcal L_B = -\nabla U(q)\cdot\nabla_p
\]

### O — Ornstein–Uhlenbeck (thermostat)
\[
\mathcal L_O
=
-\gamma p\cdot\nabla_p
+
\gamma m\beta^{-1}\Delta_p
\]

Each part corresponds to an exactly solvable dynamics.

---

## 8. Generator Exponentials → State Updates

For any sub-generator:
\[
(e^{\Delta t \mathcal L_i} f)(q,p)
=
\mathbb E[f(\Phi_i^{\Delta t}(q,p))]
\]

Thus, exponentials of generators act by **moving \((q,p)\)**.

---

## 9. BAOAB Algorithm

### Operator splitting
\[
e^{\Delta t\mathcal L}
\approx
e^{\frac{\Delta t}{2}\mathcal L_B}
e^{\frac{\Delta t}{2}\mathcal L_A}
e^{\Delta t\mathcal L_O}
e^{\frac{\Delta t}{2}\mathcal L_A}
e^{\frac{\Delta t}{2}\mathcal L_B}
\]

### Concrete updates

1. **B (half force)**
\[
p \leftarrow p - \tfrac{\Delta t}{2}\nabla U(q)
\]

2. **A (half drift)**
\[
q \leftarrow q + \tfrac{\Delta t}{2m}p
\]

3. **O (OU thermostat)**

4. **A (half drift)**

5. **B (half force)**

---

## 10. OU Thermostat — Exact Solution

### OU SDE
\[
dp_t = -\gamma p_t\,dt + \sqrt{2\gamma m\beta^{-1}}\,dW_t
\]

### Integrating factor method
\[
d(e^{\gamma t}p_t) = \sigma e^{\gamma t}dW_t
\]

### Exact update
\[
\boxed{
p_{t+\Delta t}
=
e^{-\gamma\Delta t}p_t
+
\sqrt{m\beta^{-1}(1-e^{-2\gamma\Delta t})}\;\xi
}
\]

where
\[
\xi \sim \mathcal N(0,1)
\]

---

## 11. Origin of the Gaussian Random Variable

- Brownian increments are Gaussian
- Itô integrals of deterministic functions are Gaussian
- Variance computed via **Itô isometry**
- Any Gaussian with variance \(v\) can be written as \(\sqrt v\,\xi\)

Thus the entire Brownian path over one timestep collapses into **one Gaussian draw**.