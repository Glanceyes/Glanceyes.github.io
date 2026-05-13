---
title: AutoEncoder · VAE · GAN
subtitle: Brief notes prepared for technical interviews
slug: autoencoder-vae-gan
order: 10
category: Interview Preparation
topic_chips:
  - AutoEncoder
  - VAE / VQ-VAE
  - GAN / WGAN
  - Generative Metrics
pdf: /pdf/archives/10-autoencoder-vae-gan.pdf
thumb: /images/archives/notes/10-autoencoder-vae-gan-p44-autoencoder.png
excerpt_short: AutoEncoders, VAEs, GANs, and generative model evaluation metrics
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., Krafton, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover the family of latent-variable and adversarial generative models: how to compress data into representations, how to make those representations probabilistic, and how to train generators by competing against a discriminator.

## AutoEncoder

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p44-autoencoder.png" alt="AutoEncoder">

- neural network that learns a compressed (compact) representation of data

**Architecture**

- **Encoder**: maps input to latent space
- **Decoder**: reconstructs the input from the latent code

**Objective**

- minimize reconstruction error

**Deterministic latent representation**

- no explicit probabilistic modeling → not a generative model
  - does not define a valid generative distribution
- random sampling in latent space is unreliable
  - sampling random $z$ does not guarantee meaningful outputs

## Variational AutoEncoder (VAE)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p44-vae-p44.png" alt="VAE (page 44 portion)">

- imposes a probabilistic structure on the latent space → learns a latent distribution directly

**Encoder**

- $q_\phi(z \mid x) = \mathcal{N}(\mu_\phi(x), \sigma_\phi^2(x))$
- approximate posterior: distribution of latent $z$ given by $x$
- outputs statistics of the distribution

**Decoder**

- $p_\theta(x \mid z)$
- likelihood: generative distribution of $x$ given by latent $z$

**Reparameterization Trick**

- separates randomness from parameters → enables backpropagation
- $z = \mu_\phi(x) + \sigma_\phi(x) \odot \epsilon$, $\epsilon \sim \mathcal{N}(0, I)$

### Objective

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p45-vae-p45-cont.png" alt="VAE Objective (handwritten)">

- maximize the marginal log-likelihood of the data: $\log p(x) = \log \int p_\theta(x, z) \, dz$
- intractable in general: the posterior $p(z \mid x)$ is intractable
- → motivates the **ELBO** (variational lower bound)

### ELBO (Evidence Lower BOund)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p45-elbo.png" alt="ELBO derivation (handwritten)">

- direct maximization of $\log p(x) = \log \int p_\theta(x, z) \, dz$ is intractable
- maximize ELBO instead:

    $$
    \log p_\theta(x) \geq \mathbb{E}_{q_\phi(z \mid x)}[\log p_\theta(x \mid z)] - \text{KL}\!\left( q_\phi(z \mid x) \,\big\|\, p(z) \right)
    $$

- **Reconstruction term**
  - encourages accurate reconstruction → keep latent informative
- **KL regularization term**
  - pushes posterior toward prior (e.g., Gaussian distribution) → smooth ↑
    - preventing posterior from Dirac delta distribution

**Drawbacks**

- blurry samples & posterior collapse
  - due to Gaussian likelihood and averaging
- continuous latent space → not ideal for discrete structure

### Vector-Quantized VAE (VQ-VAE)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p45-vq-vae.png" alt="VQ-VAE">

- replace continuous latent $z$ with discrete codebook entries
  - encoder → outputs a vector
  - vector → quantized to nearest codebook vector
- objective

  $$
  \mathcal{L} = \|x - \hat{x}\|^2 + \|\operatorname{sg}[z_e] - e\|^2 + \beta \|z_e - \operatorname{sg}[e]\|^2
  $$

  - reconstruction loss + codebook loss + commitment loss
- captures discrete structure

## Generative Model

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p46-gen-model.png" alt="Generative Model">

- **Push-forward mapping**
  - learn a mapping that pushes a simple distribution into a complex data distribution
- two viewpoints
  - **density-based (likelihood)**: model $p_\theta(x)$ directly (e.g., flows)
  - **implicit (sampling)**: model a generator that can sample but may not give tractable likelihood (e.g., GANs)

### Evaluation Metrics

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p46-eval-metrics-intro.png" alt="Evaluation Metrics intro">

- low-level: perceptual metric
- high-level: compare real and generated sample distributions in a feature space
  - metrics often rely on pretrained networks (Inception) as a feature extractor
- caveats
  - not always aligned with human preference
  - sensitive to the feature extractor and dataset domain shift

#### Peak Signal-to-Noise Ratio (PSNR)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p46-psnr-p46.png" alt="PSNR (page 46 portion)">

$$
\text{MSE} = \frac{1}{N} \sum_{i=1}^{N} (x_i - \hat{x}_i)^2, \qquad \text{PSNR} = 10 \log_{10}\!\left(\frac{\text{MAX}^2}{\text{MSE}}\right)
$$

- pixel-level reconstruction fidelity
  - based on MSE (mean squared error)
  - MAX: maximum possible pixel value (e.g., 255 or 1)
- higher is better (MSE lower)
- limitation
  - sensitive to small pixel shifts and blur
  - can favor over-smoothed results that look less sharp to humans

#### Structural Similarity Index (SSIM)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p47-ssim.png" alt="SSIM">

$$
\text{SSIM}(x, \hat{x}) = l(x, \hat{x}) \cdot c(x, \hat{x}) \cdot s(x, \hat{x})
$$

- similarity of local structure rather than exact pixel match
  - compares luminance, contrast, and structure
- high-level form
  - typically computed on local windows and averaged
  - often correlates with perceived quality better than PSNR for some tasks
- higher is better
- limitation
  - still not a full perceptual metric
  - may not reflect semantic realism in generative outputs

#### Fréchet Inception Distance (FID)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p47-fid.png" alt="FID">

$$
\text{FID} = \|\mu_r - \mu_g\|^2 + \text{Tr}\!\left(\Sigma_r + \Sigma_g - 2(\Sigma_r \Sigma_g)^{1/2}\right)
$$

- distance between real and generated distributions in Inception feature space
  - approximates each feature distribution as a Gaussian
- extract features (often from Inception pool3)
  - compute mean (quality) and covariance (diversity)
  - real: $(\mu_r, \Sigma_r)$
  - generated: $(\mu_g, \Sigma_g)$
- lower is better
- known failure modes
  - biased for small sample sizes (needs enough samples)
  - can be gamed if feature extractor is inappropriate

#### Inception Score

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p48-fid-continued.png" alt="FID continued (page 48 top)">

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p48-inception-score.png" alt="Inception Score">

$$
\text{IS} = \exp\!\left(\mathbb{E}_x[\text{KL}(p(y \mid x) \,\|\, p(y))]\right), \qquad p(y) = \mathbb{E}_x[p(y \mid x)]
$$

- uses a pretrained classifier's outputs $p(y \mid x)$
  - confident predictions for each image (low entropy per image)
    - quality proxy: $p(y \mid x)$ is sharp
  - diverse images across the set (high entropy marginal)
    - diversity proxy: $p(y)$ is broad
- higher is better
- limitations
  - does not compare to the real data distribution directly
  - can be high even when samples do not match the target dataset distribution
  - depends strongly on the classifier label space and domain

#### Learned Perceptual Image Patch Similarity (LPIPS)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p48-lpips-p48.png" alt="LPIPS (page 48 portion)">

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p49-lpips-p49.png" alt="LPIPS (page 49 portion)">

$$
\text{LPIPS}(x, \hat{x}) = \sum_l w_l \, \|\hat{\phi}_l(x) - \hat{\phi}_l(\hat{x})\|_2^2
$$

- perceptual distance using deep features from a pretrained network
  - compares images in feature space rather than pixel space
- definition
  - extract multi-layer features $\phi_l(x)$ and $\phi_l(\hat{x})$
  - compute weighted feature differences across layers
  - $\hat{\phi}$: channel-normalized features
- interpretation
  - lower is better (more perceptually similar)
  - commonly used for image-to-image translation, restoration, diffusion reconstruction quality
- limitation
  - depends on the backbone and training domain
  - not a distribution metric (unlike FID): it's a pairwise similarity metric

## Generative Adversarial Network (GAN)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p49-gan.png" alt="GAN / WGAN (handwritten)">

- implicit generative model (no explicit likelihood)
  - generator $G$: $z \sim p(z) \to \hat{x} = G(z)$
  - discriminator $D$: outputs probability real for an input image

**Adversarial training**

- $D$ learns to distinguish real v.s. fake
- $G$ learns to fool $D$
- train as a minimax game

**Objective (standard)**

- $$\min_G \max_D \; \mathbb{E}_{x \sim p_{\text{data}}}[\log D(x)] + \mathbb{E}_{z \sim p(z)}[\log(1 - D(G(z)))]$$
- common generator variant (stronger gradients early in training)
  - $\mathcal{L}_G = -\mathbb{E}_z[\log D(G(z))]$: non-saturating loss

**Strengths**

- sharp, high-quality samples
- fast sampling (single forward pass)

**Weaknesses**

- training instability
- mode collapse (diversity ↓)

### Wasserstein GAN (WGAN)

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p49-wgan-p49.png" alt="WGAN: section header, critic replacement, support-mismatch motivation (page 49 bottom)">

<img class="note-img" src="/images/archives/notes/10-autoencoder-vae-gan-p50-wgan-continued.png" alt="WGAN: Wasserstein distance proxy, Lipschitz constraint, WGAN-GP (page 50 top)">

- replace discriminator with a critic $D$ (no sigmoid)
  - optimize Wasserstein-1 distance proxy
  - critic loss: $\mathcal{L}_D = \mathbb{E}[f(\hat{x})] - \mathbb{E}[f(x)]$
  - enforce Lipschitz constraint (weight clipping or gradient penalty)
    - **WGAN-GP**: gradient penalty on the critic
- mitigates issues where the supports of $p_{\text{data}}$ and $p_g$ don't overlap → unstable generator training

