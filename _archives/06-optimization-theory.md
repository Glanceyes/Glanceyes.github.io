---
title: Optimization Theory
subtitle: Brief notes prepared for technical interviews
slug: optimization-theory
order: 6
category: Interview Preparation
topic_chips:
  - Convexity
  - Lipschitz Smoothness
  - Lagrangian Duality
pdf: /pdf/archives/06-optimization-theory.pdf
thumb: /images/archives/thumbs/06-optimization-theory.png
excerpt_short: Convexity, Lipschitz smoothness, and constrained optimization via Lagrangian duality
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., KRAFTON, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover the theoretical conditions that make optimization problems tractable: when local solutions are global (convexity), what smoothness lets us bound step sizes, and how constrained problems are reformulated via duality.

## Convexity

<img class="note-img" src="/images/archives/notes/06-optimization-theory-p31-convexity-intro.png" alt="Convexity intro">

- determines whether a problem has a single global minimum
  - a property of functions and optimization problems

**Intuition**

- a function is convex if the line segment between any two points lies above the function
- no local minima other than the global minimum

### Convex Function

<img class="note-img" src="/images/archives/notes/06-optimization-theory-p32-convex-function.png" alt="Convex Function (with Jensen inequality)">

- a function $f$ is convex if for all $x, y$ and $\lambda \in [0, 1]$:

    $$
    f(\lambda x + (1-\lambda) y) \leq \lambda f(x) + (1-\lambda) f(y)
    $$

  - this is **Jensen's inequality** in its discrete two-point form
  - more generally: $f(\mathbb{E}[X]) \leq \mathbb{E}[f(X)]$ for any random variable $X$
- geometric interpretation
  - bowl-shaped surface
  - any descent direction eventually leads to the same minimum
  - **any local minimum is a global minimum**
  - gradient descent is guaranteed to converge (with proper step size)

### Hessian-Based Characterization

<img class="note-img" src="/images/archives/notes/06-optimization-theory-p32-hessian-based.png" alt="Hessian-Based Characterization">

- for twice-differentiable functions
  - $f$ is convex if its Hessian is positive semi-definite
    - $H \succeq 0$ → convex
- intuition
  - eigenvalues of the Hessian represent curvature
  - all eigenvalues $\geq 0$ → no direction curves downward
- example
  - in linear regression loss
    - Hessian $= 2 X^\top X$
    - $\nabla_w \mathcal{L} = -\frac{2}{N} X^\top (y - Xw)$
    - always positive semi-definite

### Why Convexity Matters in ML

<img class="note-img" src="/images/archives/notes/06-optimization-theory-p32-why-convexity.png" alt="Why convexity matters in ML">

- guarantees a unique global optimum
  - avoids issues like local minima, saddle points, etc.
- simplifies optimization and theoretical analysis

**v.s. non-convex problems**

- neural networks, matrix factorization, deep generative models, etc.
- does not mean impossible
  - but optimization guarantees are weaker
- convex ≠ easy: large-scale convex problems can still be expensive

## Lipschitz Continuity

<img class="note-img" src="/images/archives/notes/06-optimization-theory-p33-lipschitz.png" alt="Lipschitz Continuity & Smoothness">

- a notion of smoothness of a function
  - controls how fast a function can change

**Definition**

- $f$ is Lipschitz continuous if there exists $L \geq 0$ such that

$$
\|f(x) - f(y)\| \leq L \, \|x - y\| \quad \forall x, y
$$

**Intuition**

- the function has a bounded slope
- $L$ is the maximum rate of change
- prevents the function from changing too abruptly

### Lipschitz Continuous Gradient (Smoothness)

<img class="note-img" src="/images/archives/notes/06-optimization-theory-p33-lipschitz-gradient.png" alt="Lipschitz Continuous Gradient (Smoothness)">

- a differentiable function $f$ has an $L$-Lipschitz continuous gradient if

$$
\|\nabla f(x) - \nabla f(y)\| \leq L \|x - y\|
$$

**Interpretation**

- the gradient does not change too fast
- the curvature of the function is bounded
- equivalently, the Hessian is bounded above in Loewner order: $\nabla^2 f(x) \preceq L I$

**Why Lipschitz continuity matters in optimization**

- guarantees stability of gradient descent
- allows choosing a safe learning rate
  - if gradient is $L$-Lipschitz: step size $\eta \leq \frac{1}{L}$ ensures descent
- provides convergence guarantees for first-order methods

## Lagrangian Duality

<img class="note-img" src="/images/archives/notes/06-optimization-theory-p23-lagrangian-duality.png" alt="Lagrangian Duality (handwritten)">

- constrained optimization problems are hard to solve directly with first-order methods (gradient zero is not enough when constraints are active)
- duality reformulates a problem in different variables, often easier to optimize

**Constrained optimization problem**

$$
\min_x f(x) \quad \text{s.t.} \quad g(x) \leq 0, \quad h(x) = 0
$$

**Lagrangian**

$$
\mathcal{L}(x, \lambda, \nu) = f(x) + \sum_i \lambda_i g_i(x) + \sum_j \nu_j h_j(x), \quad \lambda_i \geq 0
$$

- $\lambda, \nu$ are Lagrangian multipliers

**Dual function**

$$
D(\lambda, \nu) = \inf_x \mathcal{L}(x, \lambda, \nu)
$$

- lower bound on the optimal value of the primal problem
- the dual problem maximizes $D$ subject to $\lambda \geq 0$

**Conjugate Prior (Bayesian context)**

- to obtain a posterior in closed form given a likelihood family
- choose a prior family such that the posterior stays in the same family
- $p(\theta \mid x) \propto p(x \mid \theta) p(\theta)$
- e.g., Gaussian-Gaussian-Gaussian conjugacy
