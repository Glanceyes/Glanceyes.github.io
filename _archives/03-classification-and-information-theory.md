---
title: Classification & Information Theory
subtitle: Linear classifiers, probability calibration, and information-theoretic losses.
slug: classification-and-information-theory
order: 3
category: Interview Preparation
topic_chips:
  - SVM
  - Logit & Softmax
  - Entropy
  - Cross-Entropy / KL
pdf: /pdf/archives/03-classification-and-information-theory.pdf
thumb: /images/archives/notes/03-classification-and-information-theory-p11-svm-intro.png
excerpt_short: SVM (hard/soft margin & kernel trick), logits and softmax, entropy, cross-entropy and KL divergence.
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., Krafton, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover the foundations of discriminative classification and the information-theoretic objectives that drive most learning losses.

## Support Vector Machine (SVM)

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p11-svm-intro.png" alt="SVM overview">

- find a decision boundary with the maximum margin
  - margin: distance from the boundary to the closest training points
- supervised learning model for classification (and regression as SVR)
- linear decision function
  - $f(x) = w^\top x + b$
  - predict by $\operatorname{sign}(f(x))$

### Maximum-Margin (Hard Margin)

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p11-svm-hard.png" alt="Hard-margin SVM (handwritten)">

- separable case
  - constraints: $y_i(w^\top x_i + b) \geq 1$
  - maximize margin $\frac{2}{\|w\|}$ is equivalent to minimizing $\frac{1}{2}\|w\|^2$
- primal optimization:

    $$
    \min_{w,b} \tfrac{1}{2}\|w\|^2 \quad \text{s.t.} \quad y_i(w^\top x_i + b) \geq 1
    $$

- **Support vectors**
  - the points that lie on the margin boundaries
  - they "support" the optimal hyperplane
  - only these points determine the solution

### Soft Margin (Non-Separable)

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p11-svm-soft-p11.png" alt="Soft-margin SVM (page 11 portion)">

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p12-svm-soft-p12.png" alt="Soft-margin SVM (page 12 portion)">

- allow violations via slack variables $\xi_i \geq 0$
  - constraints: $y_i(w^\top x_i + b) \geq 1 - \xi_i$
- objective:

    $$
    \min_{w,b,\xi} \tfrac{1}{2}\|w\|^2 + C \sum_i \xi_i
    $$

- **$C$ controls tradeoff**
  - $C \uparrow$ → fewer violations, tighter fit (variance ↑)
  - $C \downarrow$ → larger margin, more tolerance (bias ↑)
- hinge loss view:

    $$
    \min_{w,b} \tfrac{1}{2}\|w\|^2 + C \sum_i \max(0, 1 - y_i f(x_i))
    $$

- margin violations are penalized linearly

### Kernel Trick

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p12-svm-kernel.png" alt="Kernel Trick (handwritten)">

- handle nonlinear boundaries by mapping inputs to a higher-dimensional feature space
  - explicit mapping $\phi(x)$ is not computed
  - use kernel $K(x, x') = \phi(x)^\top \phi(x')$
- dual form depends only on dot products
  - prediction:
    - $f(x) = \sum_{i \in \text{SV}} \alpha_i y_i K(x_i, x) + b$
- common kernels
  - linear: $K(x, x') = x^\top x'$
  - RBF: $K(x, x') = \exp(-\gamma \|x - x'\|^2)$
  - polynomial: $K(x, x') = (x^\top x' + c)^d$

## Logit & Softmax

### Logit

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p14-logit.png" alt="Logit">

$$
\text{logit}(p) = \log \frac{p}{1 - p}
$$

- logarithm of the odds of the event
  - raw & unnormalized scores output by the model
  - pre-softmax outputs whose differences correspond to log-odds between classes
- interpreted as evidence for each class

### Softmax

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p14-softmax.png" alt="Softmax (page 14)">

$$
p_i = \frac{e^{z_i}}{\sum_j e^{z_j}}
$$

- convert logits into a probability distribution
  - output is positive and sums to 1
  - preserve relative differences between logits
- smooth and differentiable
  - compatible with likelihood-based objectives
- $e^{z}$ grows extremely fast
  - if some logit $z$ is large, $e^{z}$ can exceed floating-point range → overflow (`inf`)

### Log-Sum-Exp Trick

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p15-log-sum-exp.png" alt="Log-Sum-Exp Trick (page 15)">

$$
\log \sum_j e^{z_j} = c + \log \sum_j e^{z_j - c}, \quad c = \max_j z_j
$$

- subtract the maximum logit to avoid overflow
  - keeps exponentials in a safe range

### Gumbel Softmax

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p15-gumbel-softmax.png" alt="Gumbel Softmax (page 15)">

$$
y_i = \frac{\exp\!\left((\log p_i + g_i) / \tau\right)}{\sum_j \exp\!\left((\log p_j + g_j) / \tau\right)}
$$

- differentiable approximation to categorical sampling
  - sampling from a categorical distribution is non-differentiable
  - differentiable with respect to logits
- use cases
  - discrete latent variables
  - VQ-VAE style models

## Entropy

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p15-entropy.png" alt="Entropy">

- measure uncertainty of a distribution
- defined as the expected amount of surprise
  - surprisal of an event: $-\log p(x)$
- entropy ↑ → uncertainty ↑ → ≃ uniform distribution
- entropy ↓ → more confident or peaked distribution → ≃ deterministic distribution

$$
H(p) = -\sum_i p_i \log p_i
$$

### Cross-Entropy

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p16-cross-entropy.png" alt="Cross-Entropy (page 16)">

$$
H(p, q) = -\sum_i p_i \log q_i
$$

- measures the expected surprisal when
  - true distribution is $p$
  - but outcomes are encoded using model distribution $q$
  - how "surprised" we are when using $q$ to explain data from $p$
- equivalent to negative log-likelihood
  - used as the standard loss for classification
  - penalizes assigning low probability to true labels

### KL Divergence

<img class="note-img" src="/images/archives/notes/03-classification-and-information-theory-p16-kl-divergence.png" alt="KL Divergence (page 16)">

$$
\text{KL}(p \| q) = \sum_i p_i \log \frac{p_i}{q_i} = H(p, q) - H(p)
$$

- measure how much information is lost when $q$ is used to approximate $p$
  - extra surprise due to mismatch between $p$ and $q$
- minimizing cross-entropy ⇔ minimizing KL divergence
  - since $H(p)$ is fixed
    - entropy $H(p)$: intrinsic uncertainty of the data
