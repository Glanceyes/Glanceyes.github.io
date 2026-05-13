---
title: Probability & Bayesian Inference
subtitle: Brief notes prepared for technical interviews
slug: probability-and-bayesian-inference
order: 5
category: Interview Preparation
topic_chips:
  - Bayes Theorem
  - MLE / MAP
  - Common Distributions
  - CLT
pdf: /pdf/archives/05-probability-and-bayesian-inference.pdf
thumb: /images/archives/notes/05-probability-and-bayesian-inference-p21-bayes-theorem.png
excerpt_short: Bayes' theorem, MLE/MAP, and the canonical distributions used in ML
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., Krafton, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover the probabilistic foundations of ML — how to model uncertainty, update beliefs from data, and choose between maximum-likelihood and Bayesian objectives.

## Bayes Theorem

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p21-bayes-theorem.png" alt="Bayes Theorem (page 21)">

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p22-bayes-theorem-p22-cont.png" alt="Bayes Theorem continuation (page 22 top)">

$$
p(\theta \mid x) = \frac{p(x \mid \theta) \, p(\theta)}{p(x)}
$$

- reveals how to update our belief about model parameters $\theta$ after observing data $x$
  - $p(x \mid \theta)$: likelihood
  - $p(\theta)$: prior
  - $p(\theta \mid x)$: posterior
  - $p(x)$: marginal likelihood (evidence)

### Likelihood

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p22-likelihood.png" alt="Likelihood">

- a function of parameters
  - measures how well a set of parameters $\theta$ explains the observed data
  - not a probability distribution over the data $x$
- **Gaussian model**: $x \sim \mathcal{N}(\mu, \sigma^2)$
  - likelihood: $p(x \mid \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\!\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$

### Maximum Likelihood Estimation (MLE)

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p22-mle.png" alt="MLE">

- choose parameters that maximize the likelihood of the observed data
  - without any prior belief
  - how to define the objective
    - not an optimization method itself (e.g., gradient descent)
- usually maximize log-likelihood: $\arg\max_\theta \log p(x \mid \theta)$
  - usually corresponds to minimizing a negative log-likelihood loss
    - e.g., MSE, cross-entropy

### Maximum A Posteriori (MAP)

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p22-map.png" alt="MAP">

$$
\hat{\theta}_{\text{MAP}} = \arg\max_\theta \, p(x \mid \theta) \, p(\theta)
$$

- similar to MLE but incorporates a prior belief about parameters $\theta$
  - also likely under a prior distribution (regularization)
- if prior is uniform → MAP reduces to MLE

## Probability Distributions

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p26-dist-intro.png" alt="Probability Distributions intro">

- modeling uncertainty of random variables
- mapping outcomes to probabilities
- discrete vs continuous distributions
- PMF (probability mass function) vs PDF (probability density function)

### Bernoulli

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p26-bernoulli.png" alt="Bernoulli">

$$
P(X = x) = p^x (1 - p)^{1 - x}, \quad x \in \{0, 1\}
$$

- single binary trial (e.g., success / failure)
  - $X \in \{0, 1\}$
  - success probability $p$

### Binomial

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p26-binomial-p26.png" alt="Binomial (page 26)">

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p27-binomial-p27.png" alt="Binomial statistics (page 27)">

$$
P(X = k) = \binom{n}{k} p^k (1 - p)^{n - k}
$$

- number of successes $k$ in a fixed number of trials $n$
  - independent Bernoulli trials
  - success probability $p$
- **Statistics**
  - mean: $np$
  - variance: $np(1-p)$
  - normal approximation when $n$ large

### Multinomial

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p27-multinomial.png" alt="Multinomial">

$$
P(\mathbf{X} = \mathbf{x}) = \frac{n!}{x_1! \cdots x_K!} \prod_{i=1}^{K} p_i^{x_i}
$$

- generalization of Bernoulli / Binomial to multiple categories
- used in multi-class classification
  - categorical likelihood
  - softmax output

### Normal (Gaussian)

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p27-normal.png" alt="Normal (Gaussian)">

- mean $\mu$, variance $\sigma^2$

$$
\mathcal{N}(x; \mu, \sigma^2) = \frac{1}{\sqrt{2\pi}\sigma} \exp\!\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

- commonly used as a noise model in regression

### Exponential

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p27-exponential-p27.png" alt="Exponential (page 27, with handwritten annotations)">

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p28-exponential-p28.png" alt="Exponential (page 28 portion)">

- models waiting time until the next event
- time between events in a Poisson process
- rate $\lambda$
- mean: $1/\lambda$, variance: $1/\lambda^2$
- **memoryless property**: future waiting time independent of the past

### Poisson

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p28-poisson.png" alt="Poisson">

$$
P(X = k) = \frac{\lambda^k e^{-\lambda}}{k!}
$$

- event counts over time or space
  - unknown number of trials
- $X \in \{0, 1, 2, \ldots\}$
- event rate $\lambda$
- Binomial limit when $n \to \infty,\, np = \lambda$
- independent event assumption
- mean = variance = $\lambda$

### Gamma

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p28-gamma.png" alt="Gamma">

$$
p(x) = \frac{\lambda^k}{\Gamma(k)} x^{k-1} e^{-\lambda x}, \quad x \geq 0
$$

- generalization of the Exponential distribution
- models waiting time until the $k$-th event
  - shape $k$
  - rate $\lambda$
- mean: $k / \lambda$
- variance: $k / \lambda^2$
- when $k = 1$ → Gamma = Exponential

### Central Limit Theorem (CLT)

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p28-clt-p28.png" alt="CLT (page 28 portion)">

<img class="note-img" src="/images/archives/notes/05-probability-and-bayesian-inference-p29-clt-p29.png" alt="CLT statement (page 29 portion)">

- as the number of samples $n$ increases
- the distribution of the sample mean (after normalization) converges to a Gaussian distribution

$$
\frac{1}{n}\sum_{i=1}^n X_i \xrightarrow{d} \mathcal{N}\!\left(\mu, \frac{\sigma^2}{n}\right) \quad \text{as } n \to \infty
$$
