---
title: Regression
subtitle: Brief notes prepared for technical interviews
slug: regression
order: 7
category: Interview Preparation
topic_chips:
  - Linear Regression
  - Logistic Regression
  - MLE / NLL / BCE
pdf: /pdf/archives/07-regression.pdf
thumb: /images/archives/notes/07-regression-p29-linear-regression.png
excerpt_short: Linear and logistic regression: closed-form vs gradient-based solutions, MLE perspective, and BCE
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., Krafton, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover two foundational regression models — the linear case for continuous targets and the logistic case for binary classification — and the probabilistic perspectives that connect them to MLE and MAP.

## Linear Regression

<img class="note-img" src="/images/archives/notes/07-regression-p29-linear-regression.png" alt="Linear Regression — model & objective">

<img class="note-img" src="/images/archives/notes/07-regression-p30-linear-reg-continued.png" alt="Linear Regression — closed-form, probabilistic interpretation">

- models the relationship between input features and a target variable
  - assumes a linear relationship between inputs and output

**Model**

$$
y = w^\top x + \epsilon, \quad \epsilon \sim \mathcal{N}(0, \sigma^2)
$$

- $x$: input features
- $w$: weight vector
- $\epsilon$: noise term (usually assumed Gaussian)

**Objective**

- minimize prediction error between model output and ground-truth targets
- typically using mean squared error (MSE)

$$
\mathcal{L}_{\text{MSE}} = \frac{1}{n} \sum_i (y_i - w^\top x_i)^2
$$

**Interpretation**

- each weight $w_j$ represents the contribution of feature $x_j$ to the output
- learns a hyperplane in feature space

**Gradient-based optimization**

- compute gradient of MSE w.r.t. $w$
  - $\nabla_w \mathcal{L} = -\frac{2}{N} X^\top (y - Xw)$
- update using gradient descent
  - $w_{t+1} = w_t - \eta\, \nabla_w \mathcal{L}(w_t)$

**Closed-form solution (normal equation)**

- $w^* = (X^\top X)^{-1} X^\top y$
  - derived by setting gradient to zero
- but $X^\top X$ may be non-invertible (numerical instability)
  - in practice: gradient descent, SVD, etc.

**Probabilistic interpretation**

- linear regression with MSE = MLE under Gaussian noise
  - assume Gaussian noise: $\epsilon \sim \mathcal{N}(0, \sigma^2)$
    - target: a linear function of the input plus additive noise
  - likelihood: $p(y \mid X, w) = \mathcal{N}(Xw, \sigma^2 I)$
  - maximizing log-likelihood = minimizing MSE
    - $\log p(y \mid X, w) = -\frac{1}{2\sigma^2} \|y - Xw\|^2 + \text{const}$
  - L2 regularization → MAP with Gaussian prior
    - $p(w) = \mathcal{N}(0, \tau^2 I)$

**Limitations**

- cannot model nonlinear relationships
  - unless features are manually engineered
- sensitive to outliers (due to squared error)

## Logistic Regression

<img class="note-img" src="/images/archives/notes/07-regression-p30-logistic-start.png" alt="Logistic Regression — model, classifier, decision boundary (page 30 portion)">

<img class="note-img" src="/images/archives/notes/07-regression-p31-logistic-continued.png" alt="Logistic Regression — likelihood, MLE, BCE, gradient, training (page 31)">

- linear classifier that models the probability of a binary label
- uses a linear score $w^\top x$ and squashes it into $[0, 1]$ with a sigmoid
- decision boundary: predict class 1 when $\sigma(w^\top x) > 0.5$, equivalent to $w^\top x > 0$

**Model**

$$
p(y = 1 \mid x; w) = \sigma(w^\top x) = \frac{1}{1 + e^{-w^\top x}}
$$

**Likelihood (Bernoulli)**

- $p(y \mid x, w) = p^y (1-p)^{1-y}$ where $p = \sigma(w^\top x)$
- $p(y \mid x, w) = \sigma(w^\top x)^y \, (1 - \sigma(w^\top x))^{1-y}$

**MLE Objective**

- maximize likelihood over data
- equivalent to minimizing NLL (negative log-likelihood)

**NLL = Binary Cross-Entropy (BCE)**

- $-\log p(y \mid x, w) = -y \log p - (1-y) \log (1 - p)$
- equivalent to Binary Cross Entropy (BCE)
- gradient: $\nabla_w \mathcal{L}(w) = X^\top (p - y)$

**Training**

- no closed-form solution in general
- optimize with gradient descent or variants (SGD, Adam)

