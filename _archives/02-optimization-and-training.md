---
title: Optimization & Training
subtitle: How parameters are updated and gradients are propagated in deep networks.
slug: optimization-and-training
order: 2
category: Interview Preparation
topic_chips:
  - Backpropagation
  - Optimizers
  - Initialization
  - Activations
pdf: /pdf/archives/02-optimization-and-training.pdf
thumb: /images/archives/notes/02-optimization-and-training-p06-backpropagation.png
excerpt_short: Backpropagation, optimizers (SGD/Momentum/RMSProp/Adam), activations, and initialization.
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., Krafton, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover the mechanics that make deep networks trainable: how gradients are computed, how parameters are updated efficiently, how to choose initial values, and how activations shape gradient flow.

## Backpropagation

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p06-backpropagation.png" alt="Backpropagation (page 6 portion)">

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p07-backprop-continued.png" alt="Backpropagation continued — Chain rule, Computation flow, Dynamic programming (page 7 portion)">

- efficiently compute gradients of a loss with respect to all parameters
  - based on the chain rule
  - enables training of deep neural networks via gradient-based optimization
- propagate error signals from the output layer backward through the network

**Chain rule**

- reusing intermediate derivatives → avoid redundant computations

**Computation flow**

- forward pass: compute activations and loss
- backward pass: compute gradients layer by layer from output to input
- each layer contributes a local derivative → gradients = products of local derivatives

**Dynamic programming**

- cache intermediate activations during the forward pass
- reuse activations during the backward pass to compute gradients efficiently
- complexity: linear in the number of parameters

### Gradient Accumulation

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p07-gradient-accumulation.png" alt="Gradient Accumulation">

- simulate a larger batch size by accumulating gradients over multiple forward/backward passes
  - delays parameter updates until enough gradients are accumulated
- useful for overcoming GPU memory limits

### Gradient Checkpointing

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p07-gradient-checkpointing.png" alt="Gradient Checkpointing">

- store only a subset of activations during the forward pass → saving memory
  - decompose the network into segments with checkpoints
- recompute the rest during backpropagation
  - reruns forward computation from the nearest checkpoint when needed
- trade additional computation for reduced memory usage

## Optimizer

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p08-optimizer-intro.png" alt="Optimizer">

- update model parameters to minimize a loss function
- determine update direction and step size

### Gradient Descent

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p08-gradient-descent.png" alt="Gradient Descent">

$$
\theta_{t+1} = \theta_t - \eta\, \nabla \mathcal{L}(\theta_t)
$$

- iterative optimization algorithm using the full dataset
  - update parameters in the direction of the negative gradient of the loss function
  - moving parameters toward the direction of steepest descent
- assumption: local surface is locally smooth
- computationally expensive

### Learning Rate

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p08-learning-rate.png" alt="Learning Rate">

- hyperparameter $\eta$ controlling how aggressively parameters are updated

### Step Size

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p08-step-size.png" alt="Step Size & Gradient Clipping">

- step size $= \eta \cdot \|\nabla \mathcal{L}(\theta)\|$ (learning rate × gradient magnitude)
- the actual magnitude of the parameter update
- **Gradient Clipping**
  - decouple the step size from the gradient magnitude
  - prevent exploding gradients without artificially stalling convergence
    - only reducing the learning rate may not solve the problem

### Stochastic Gradient Descent (SGD)

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p09-sgd-p9.png" alt="SGD (page 9 portion)">

- leverage a mini-batch to estimate the gradient instead of using the full dataset
  - produce a noisy estimate of the true gradient
- pros
  - faster iterations & scalable to large datasets
  - noise can help escape shallow local minima and saddle points
- cons
  - may oscillate around minima
    - especially in high-curvature directions
  - noisy updates
- computational efficiency v.s. better generalization

### Momentum

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p09-momentum.png" alt="Momentum">

- accumulate a velocity vector
  - builds speed in consistent descent directions
  - damps oscillations in steep directions → zig-zagging ↓

$$
v_{t+1} = \mu v_t + \nabla_\theta \mathcal{L}, \qquad \theta_{t+1} = \theta_t - \eta v_{t+1}
$$

### RMSProp

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p09-rmsprop.png" alt="RMSProp">

$$
s_t = \rho\, s_{t-1} + (1-\rho)\, g_t^2, \qquad \theta_{t+1} = \theta_t - \eta\, \frac{g_t}{\sqrt{s_t + \epsilon}}
$$

- track an exponential moving average of squared gradients → adaptive scaling
  - normalize gradients by recent magnitude
- small updates in high-curvature directions

### Adam

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p09-adam-p9.png" alt="Adam (page 9 portion)">

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p10-adam-p10.png" alt="Adam (page 10 portion)">

- using first and second moments of gradients
  - first moment (mean of gradients): momentum
  - second moment (variance of gradients): adaptive scaling
- fast convergence
- robust to noisy or sparse gradients

$$
m_t = \beta_1 m_{t-1} + (1-\beta_1) g_t, \quad v_t = \beta_2 v_{t-1} + (1-\beta_2) g_t^2
$$

$$
\theta_{t+1} = \theta_t - \eta \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}
$$

## Activation Function

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p10-activation-intro.png" alt="Activation Function (Sigmoid / Tanh / ReLU)">

- introduce non-linearity
- control how gradients propagate through the network
- examples
  - **Sigmoid**: $\sigma(x) = \frac{1}{1 + e^{-x}}$
    - derivative: $\sigma(x)(1 - \sigma(x))$
  - **Tanh**: $\tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}$
    - derivatives saturate near 0
    - vanishing gradient risk
      - gradients shrink exponentially
    - exploding gradients
      - uncontrollably growing gradients
      - common in RNNs
  - **ReLU**
    - derivative is 1 for positive inputs
    - mitigates vanishing gradients
    - example variants: Leaky ReLU, GELU

## Initialization

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p12-initialization-intro-p12.png" alt="Initialization (page 12 portion)">

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p13-initialization-intro-p13.png" alt="Initialization (page 13 portion)">

- choosing initial values of model parameters before training
  - affects gradient flow and training stability
  - poor initialization → vanishing or exploding gradients & failed convergence
  - during backpropagation
    - weights ↓ → gradients ↓ (shrinking)
    - weights ↑ → gradients ↑ (exploding)
- preserve variance of activations and gradients across layers
  - keep the variance of activations roughly constant

### Random Initialization

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p13-random-init.png" alt="Random Initialization">

$$
W_{ij} \sim \mathcal{N}(0, \sigma^2) \quad \text{or} \quad \mathcal{U}(-a, a)
$$

- break symmetry between neurons
  - but naive variance choices → instability ↑

### Xavier Initialization

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p13-xavier-init.png" alt="Xavier Initialization">

$$
\text{Var}(W) = \frac{2}{\text{fan\_in} + \text{fan\_out}}, \qquad W \sim \mathcal{U}\!\left(-\sqrt{\tfrac{6}{\text{fan\_in} + \text{fan\_out}}},\, \sqrt{\tfrac{6}{\text{fan\_in} + \text{fan\_out}}}\right)
$$

- balance variance of activations across layers
- assumes symmetric, saturating activations
  - e.g., tanh-like activations
  - zero-centered output → keep activations balanced
- harmonic mean on the numbers of nodes in input and output

### He (Kaiming) Initialization

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p13-he-init-p13.png" alt="He Initialization (page 13 portion)">

<img class="note-img" src="/images/archives/notes/02-optimization-and-training-p14-he-init-p14.png" alt="He Initialization (page 14 portion)">

$$
\text{Var}(W) = \frac{2}{\text{fan\_in}}, \qquad W \sim \mathcal{N}\!\left(0, \frac{2}{\text{fan\_in}}\right)
$$

- designed for ReLU-style activations (e.g., ReLU, Leaky ReLU, GELU)
- compensate for ReLU's sparsity
  - about half of activations are zero
  - scaling by 2 → preserve activation variance and gradient flow
