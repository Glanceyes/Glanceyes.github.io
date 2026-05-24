---
title: ML Fundamentals
subtitle: Brief notes prepared for technical interviews
slug: ml-fundamentals
order: 1
category: Interview Preparation
topic_chips:
  - Bias/Variance
  - Overfitting & Regularization
  - Normalization
  - Dropout
pdf: /pdf/archives/01-ml-fundamentals.pdf
thumb: /images/archives/thumbs/01-ml-fundamentals.png
excerpt_short: Core concepts behind generalization, regularization, normalization, and dropout
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., KRAFTON, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover the core ML concepts that determine how a model generalizes and trains. The sections trace the bias/variance decomposition, the regularization techniques used to control capacity, the normalization layers that stabilize training, and dropout as a stochastic regularizer.

## Bias v.s. Variance

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p01-bias-variance.png" alt="Bias v.s. Variance">

$$
\mathbb{E}\big[(y - \hat{f}(x))^2\big] = \underbrace{(\mathbb{E}[\hat{f}(x)] - f(x))^2}_{\text{bias}^2} + \underbrace{\mathbb{E}\big[(\hat{f}(x) - \mathbb{E}[\hat{f}(x)])^2\big]}_{\text{variance}} + \sigma^2
$$

- **Bias**
  - error from overly simplistic assumptions → underfitting
- **Variance**
  - error from excessive sensitivity to the training data → overfitting
- **Tradeoff**
  - bias ↓ (reducing) → variance ↑ (increasing), vice versa
  - balance (sweetspot) → generalization error ↓ (minimize)

## Overfitting & Regularization

- **Overfitting**
  - learning a noise (pattern) specific to the training data, rather than generalizable features
- **Techniques for mitigating overfitting**: regularization, dropout, early stopping, cross-validation

### Regularization

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p02-regularization-intro.png" alt="Regularization intro">

- discourage large weights → limit the model's capacity
- learned function becomes smoother → toward simpler solution (variance ↓)
- added to the loss function → less sensitive to the training data
- a small amount of bias added → variance ↓

**L2 (Ridge regression)**

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p02-l2-ridge.png" alt="L2 (Ridge regression)">

$$
\mathcal{L}_{\text{reg}} = \mathcal{L} + \lambda \|w\|_2^2
$$

- penalty proportional to the squared magnitude of the weights
- soft penalty
- Gaussian prior (MAP perspective)

**L1 (Lasso)**

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p02-l1-lasso.png" alt="L1 (Lasso)">

$$
\mathcal{L}_{\text{reg}} = \mathcal{L} + \lambda \|w\|_1
$$

- penalty on the absolute values of the weights
- hard penalty → encourages sparsity in weights
- non-differentiable at 0 → robust to outliers but unstable optimization
- Laplace prior (MAP perspective)

### Dropout (as a regularizer)

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p02-dropout-reg.png" alt="Dropout (as regularizer)">

- randomly disabling perceptrons → preventing the network from relying too heavily on specific activations
- more robust representations

### Early Stopping

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p02-early-stopping.png" alt="Early Stopping">

  - monitoring validation performance → stopping training before overfitting
  - preventing the model from fitting noise → complexity ↓

### Cross-Validation

- partition the training data into K folds → train on K−1 folds, validate on the held-out one
- less biased estimate of generalization error → tune hyperparameters without leaking the test set

## Normalization

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p02-normalization-intro.png" alt="Normalization intro (page 2)">

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p03-normalization-intro-p3.png" alt="Normalization reasons / effects (page 3 top)">

- making input features or activations have similar scales → stabilizing and accelerating optimization
- preventing features with large magnitudes from dominating gradient updates
- normalizing + scaling and shifting (with learnable parameters)

**Reasons**

- gradient magnitude $\propto$ input feature scale → update biased toward certain dimensions
- ill-conditioned Hessian
  - eigenvalues spread widely → unstable optimization

**Effects**

- convergence ↑ and sensitivity ↓ to the learning rate

**Common template**: most normalization variants follow this with different statistics:

$$
\hat{x} = \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}}, \qquad y = \gamma \hat{x} + \beta
$$

### Feature Normalization

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p03-feature-norm.png" alt="Feature Normalization">

- normalizing input features (e.g., zero mean, unit variance)
  - statistics (mean, variance) computed per feature across the training dataset
  - each feature (dimension) → more equally contributing to optimization
- no learnable parameters
- effective usage
  - commonly used as a preprocessing step

### Batch Normalization

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p03-batch-norm-p3.png" alt="Batch Normalization (page 3 portion)">

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p04-batch-norm-p4.png" alt="Batch Normalization (page 4 portion)">

- normalizing activations (outputs of the nodes) using mini-batch statistics (mean, variance)
- applied during training
  - inference: use moving-average statistics collected during training
- internal covariate shift (↓)
  - distribution of activations changes as parameters of previous layers update
  - making optimization stable and faster convergence
- learnable parameters for scaling and shifting
  - $\gamma$ (scale), $\beta$ (shift) per feature dimension (channel)
- training mechanics
  - statistics computed per channel, across batch and spatial dims
  - parameters updated via backpropagation
  - running statistics updated via exponential moving average (EMA)
- usage
  - CNNs, large-batch training

### Layer Normalization

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p04-layer-norm.png" alt="Layer Normalization">

- normalizing activations across feature dimensions, per sample
- applied during training and inference (no train/test discrepancy)
- learnable parameters for scaling and shifting
  - $\gamma$ (scale), $\beta$ (shift) per feature dimension
- effective usage
  - widely used in transformers and sequence models
    - stable for variable-length inputs
    - suitable for small-batch settings

### Instance Normalization

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p04-instance-norm-p4.png" alt="Instance Normalization (page 4 portion)">

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p05-instance-norm-p5.png" alt="Instance Normalization (page 5 portion)">

- normalizing activations per sample and per channel
  - statistics (mean, variance) computed
    - across spatial dimensions $(H, W)$
    - for each channel independently
    - for each sample independently
- applied during training and inference
  - no train-test discrepancy
  - no dependence on batch statistics
- learnable parameters for scaling and shifting
  - $\gamma$ (scale), $\beta$ (shift) for each channel
- interpretation
  - removes instance-specific contrast and illumination statistics
  - preserves content structure while normalizing style-related variations
- usage
  - image generation and style transfer
  - tasks where instance-level appearance variation is undesirable
  - small-batch or batch-size-1 settings

### Adaptive Instance Normalization (AdaIN)

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p05-adain.png" alt="Adaptive Instance Normalization (AdaIN): page 5">

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p06-adain-continued.png" alt="AdaIN: γ/β formula, properties, effective usage (page 6 top)">

- an extension of Instance Normalization
  - used for conditional normalization
- core idea
  - align the channel-wise statistics of a content feature
    - to those of a conditioning style feature
- formulation
  - given content feature $x$ and style feature $y$:
    - $\text{AdaIN}(x, y) = \sigma(y) \cdot \dfrac{x - \mu(x)}{\sigma(x)} + \mu(y)$
- interpretation
  - normalize content features using Instance Normalization
  - then inject style information via scale and shift
    - replacing $\gamma, \beta$ with style-dependent statistics
- comparison to Instance Normalization
  - Instance Norm
    - $x \mapsto \gamma \cdot \dfrac{x - \mu(x)}{\sigma(x)} + \beta$
    - $\gamma, \beta$ are learnable parameters
  - AdaIN
    - $\gamma = \sigma(y), \beta = \mu(y)$ come from the style feature at runtime
    - parameters are dynamically computed from style input
- properties
  - removes instance-specific appearance from content
  - reintroduces desired style characteristics
    - color, contrast, texture statistics
- effective usage
  - style transfer
  - image generation with controllable appearance
  - conditional generative models

## Dropout

<img class="note-img" src="/images/archives/notes/01-ml-fundamentals-p16-dropout.png" alt="Dropout">

- randomly deactivate neurons during training
  - prevent the network from relying too heavily on specific activations → overfitting ↓

