---
title: Sequence Models
subtitle: Brief notes prepared for technical interviews
slug: sequence-models
order: 9
category: Interview Preparation
topic_chips:
  - RNN / LSTM
  - Transformer
  - Attention Variants
  - Vision Transformer
pdf: /pdf/archives/09-sequence-models.pdf
thumb: /images/archives/notes/09-sequence-models-p39-transformer-intro.png
excerpt_short: From RNNs to Transformers and ViT: attention mechanisms, positional encodings, and complexity
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., Krafton, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover the evolution from recurrent to attention-based sequence models, the mechanics of self-attention, the variants used to control its cost, and the adaptation of Transformers to images via patch tokens.

## RNN

<img class="note-img" src="/images/archives/notes/09-sequence-models-p36-rnn-intro.png" alt="RNN intro (page 36 portion)">

<img class="note-img" src="/images/archives/notes/09-sequence-models-p37-rnn-intro-p37.png" alt="RNN intro (page 37 portion)">

$$
h_t = f(W_x x_t + W_h h_{t-1} + b)
$$

- sequence modeling architecture for sequential data
  - explicitly models temporal dependencies
- processes inputs step by step in time order
  - hidden state carries information across time steps
  - same weights shared across time
- order-sensitive & variable-length input handling
  - hidden state acts as a compressed summary of the past

### Backpropagation Through Time (BPTT)

<img class="note-img" src="/images/archives/notes/09-sequence-models-p37-bptt.png" alt="Backpropagation Through Time">

- unfold the RNN over time → apply backpropagation across time steps
- prone to vanishing or exploding gradients
  - gradients flow backward through many steps

### Limitations

<img class="note-img" src="/images/archives/notes/09-sequence-models-p37-rnn-limitations.png" alt="RNN Limitations">

- **Vanishing gradients**
  - long-range dependencies are hard to learn
- **Sequential computation**
  - no parallelism across time steps
- long-context modeling is inefficient

### LSTM / GRU

<img class="note-img" src="/images/archives/notes/09-sequence-models-p37-lstm-gru.png" alt="LSTM / GRU">

- introduce gating mechanisms → mitigate vanishing gradient problem
- still sequential → scalability issues remain

### RNN vs Transformer

<img class="note-img" src="/images/archives/notes/09-sequence-models-p37-rnn-vs-transformer.png" alt="RNN vs Transformer — RNN side (page 37)">

<img class="note-img" src="/images/archives/notes/09-sequence-models-p38-rnn-vs-transformer-cont.png" alt="RNN vs Transformer — Transformer side (page 38 top)">

- **RNN**
  - strong inductive bias for sequence order
  - sequential, memory-based
- **Transformer**
  - attention-based global context
  - fully parallelizable
  - better long-range dependency modeling

## Transformer

<img class="note-img" src="/images/archives/notes/09-sequence-models-p39-transformer-intro.png" alt="Transformer intro">

- sequence-to-sequence architecture based on attention
- parallel computation over sequence positions

### Encoder

<img class="note-img" src="/images/archives/notes/09-sequence-models-p39-encoder.png" alt="Encoder">

- input sequence → encoding (mapping) → contextualized representations (context-aware embeddings)
- **Self-attention**
  - each token attends to all other tokens in the input sequence
    - from the same sequence
  - captures global context within representations

### Decoder

<img class="note-img" src="/images/archives/notes/09-sequence-models-p40-decoder-p40.png" alt="Decoder">

- generate the output sequence autoregressively
- **Masked self-attention**
  - prevent attending to future tokens
  - previously generated tokens from the decoder
  - causal mask → preventing future information leakage
- **Cross-attention**
  - give attention over encoder outputs
    - allowing the decoder to condition on the input
  - query: decoder
  - key, value: encoder

### Scaled Dot-Product Attention

<img class="note-img" src="/images/archives/notes/09-sequence-models-p40-scaled-dot.png" alt="Scaled Dot-Product Attention">

$$
\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right) V
$$

**Why divide by $\sqrt{d_k}$?**

- dot products grow with dimension → divergence
- stabilizing gradients and preventing softmax saturation

### Multi-Head Attention

<img class="note-img" src="/images/archives/notes/09-sequence-models-p40-multi-head.png" alt="Multi-Head Attention">

- each head attends to different subspaces → expressiveness ↑
  - captures diverse relationships and different representation subspaces simultaneously

### Attention Complexity

<img class="note-img" src="/images/archives/notes/09-sequence-models-p40-attention-complexity.png" alt="Attention Complexity">

- computation: $O(n^2 d)$
  - for calculating $QK^\top$
- memory: $O(n^2)$ for the attention weights matrix

## Long-Context Attention Variants

<img class="note-img" src="/images/archives/notes/09-sequence-models-p41-long-context-intro.png" alt="Long-context approaches intro">

### Linear Attention

<img class="note-img" src="/images/archives/notes/09-sequence-models-p41-linear-attention.png" alt="Linear Attention">

$$
\operatorname{softmax}(Q K^\top) \approx \phi(Q)\, \phi(K)^\top
$$

- rewriting attention using a kernel feature map $\phi$
  - avoid forming $n \times n$ matrix $(QK^\top)$ → complexity ↓ (quadratic → linear)
  - at the cost of approximating softmax attention
- asymptotic complexity: $O(n d^2) \approx O(n)$ (when $n \gg d$)
  - first computing $\phi(K)^\top V \to O(n d^2)$ ($d \times d$ matrix)
  - then calculating $\phi(Q) \cdot (\cdot) \to O(n d^2)$
- drawbacks: approximated softmax attention result

### Sliding Window Attention

<img class="note-img" src="/images/archives/notes/09-sequence-models-p41-sliding-window.png" alt="Sliding Window Attention">

- each token attends only to a local window of neighboring tokens
  - limiting receptive field → reducing quadratic attention cost
- pros: preserves local context well
- cons
  - cannot directly capture long-range dependencies
  - global information may be lost

### Sparse / Flash Attention

- sparse attention (local + global mixtures)
- flash attention: I/O-aware exact attention with tiled computation

## Positional Encoding

- Transformers are permutation-invariant (no notion of order) → inject positional information
  - added to token embeddings as an additive bias

### Sinusoidal Positional Encoding

<img class="note-img" src="/images/archives/notes/09-sequence-models-p42-sinusoidal-pe.png" alt="Sinusoidal Positional Encoding">

$$
\text{PE}_{(pos, 2i)} = \sin\!\left(\frac{pos}{10000^{2i/d}}\right), \qquad \text{PE}_{(pos, 2i+1)} = \cos\!\left(\frac{pos}{10000^{2i/d}}\right)
$$

- deterministic & parameter-free
- encodes absolute position only
  - relative distance → not explicitly modeled in attention scores
- low-dimension pair → low frequency

### Rotary Positional Encoding (RoPE)

<img class="note-img" src="/images/archives/notes/09-sequence-models-p42-rope.png" alt="Rotary Positional Encoding (RoPE)">

$$
\begin{pmatrix} x'_{2i} \\ x'_{2i+1} \end{pmatrix}
= \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}
  \begin{pmatrix} x_{2i} \\ x_{2i+1} \end{pmatrix}, \qquad
z_i(p) = z_i \cdot e^{i \theta_i p}
$$

- directly and naturally encodes relative positional information
  - rotates each pair of dimensions in query and key vectors
    - represents pairs of embedding dimensions as complex numbers
  - position-dependent rotations using Euler's formula
    - dot products between rotated queries and keys
    - dependent on relative position (not absolute position)
  - not added to input token embeddings
- better extrapolation to long sequences
  - widely used in modern LLMs

## Vision Transformer (ViT)

<img class="note-img" src="/images/archives/notes/09-sequence-models-p42-vit-intro-p42.png" alt="ViT intro (page 42)">

<img class="note-img" src="/images/archives/notes/09-sequence-models-p43-vit-intro-p43.png" alt="ViT intro (page 43)">

- applies the Transformer encoder to images
  - image → sequence of patch tokens
- replaces convolution with self-attention
  - global context modeling from the first layer

**Key trade-off**

- global modeling & scalability
- weaker inductive bias → large data required
  - **Inductive bias**: what a model assumes is likely to be true about the world
    - assumptions a model makes about the structure of the data before seeing any data

### Patch Embedding

<img class="note-img" src="/images/archives/notes/09-sequence-models-p43-patch-embedding.png" alt="Patch Embedding">

- split image into fixed-size patches
- flatten + linear projection → tokens

### Positional Encoding

<img class="note-img" src="/images/archives/notes/09-sequence-models-p43-vit-positional-encoding.png" alt="ViT Positional Encoding">

- inject spatial information into patch embeddings
- required due to permutation-invariant attention

### [CLS] Token

<img class="note-img" src="/images/archives/notes/09-sequence-models-p43-cls-token.png" alt="[CLS] Token">

- prepend a learnable classification token to the patch sequence
- participates in self-attention with all patches
  - aggregates global image information across layers
- final classification head applied to the [CLS] representation
