---
title: Conditioning & Distillation
subtitle: Brief notes prepared for technical interviews
slug: conditioning-and-distillation
order: 12
category: Interview Preparation
topic_chips:
  - Knowledge Distillation
  - DINO
  - ControlNet
  - T2I Adapter
pdf: /pdf/archives/12-conditioning-and-distillation.pdf
thumb: /images/archives/thumbs/12-conditioning-and-distillation.png
excerpt_short: Knowledge distillation and conditioning adapters for pretrained diffusion models
---

> - These notes were prepared while studying for technical interviews (e.g., Snap Inc., Krafton, etc.).
> - Each entry contains a concise English summary, key math expressions, and excerpts from my original handwritten/typed study notes.

These notes cover two complementary directions for steering pretrained models: distilling knowledge from larger or self-supervised teachers, and bolting on adapter networks that inject external control signals into a frozen diffusion backbone.

## Knowledge Distillation

<img class="note-img" src="/images/archives/notes/12-conditioning-and-distillation-p55-kd-only.png" alt="Knowledge Distillation">

- transfer knowledge from a teacher model to a student model
  - student learns to match teacher outputs or intermediate features

**Why used**

- **Compression**: smaller model with similar accuracy
- **Speed**: faster inference (mobile / real-time)
- **Regularization**: teacher soft targets can improve generalization

**Common objectives**

- **Logit distillation** (soft targets)
  - $\mathcal{L} = \text{CE}(y, p_s) + \lambda T^2 \cdot \text{KL}(p_t^{(T)} \,\|\, p_s^{(T)})$
  - $p^{(T)} = \operatorname{softmax}(z / T)$
- **Feature distillation**
  - match hidden representations (e.g., L2 / cosine loss)

**Key knobs**

- temperature $T$ (softness of targets)
- weight $\lambda$ (balance with supervised loss)

### DINO

<img class="note-img" src="/images/archives/notes/12-conditioning-and-distillation-p55-dino-p55.png" alt="DINO start (handwritten, page 55)">

<img class="note-img" src="/images/archives/notes/12-conditioning-and-distillation-p56-dino-p56.png" alt="DINO continued (handwritten, page 56)">

- self-supervised distillation for vision transformers (teacher-student)
  - student learns to predict the teacher's output distribution
  - no labels required

**Core idea**

- two augmented views of the same image
- teacher sees global crops, student sees global + local crops
- teacher targets are sharpened (low entropy) with temperature scheduling
- model learns local-to-global correspondence

**Objective**

- minimize cross-entropy between teacher and student outputs
- encourages consistent representations across views

**Why it works**

- teacher is an EMA of the student (stable target)
- centering + temperature prevent collapse (trivial constant outputs)

**Outcome**

- learns strong visual features that transfer well to downstream tasks

## ControlNet

<img class="note-img" src="/images/archives/notes/12-conditioning-and-distillation-p56-controlnet.png" alt="ControlNet (page 56: core idea, how it works, benefits)">

<img class="note-img" src="/images/archives/notes/12-conditioning-and-distillation-p57-controlnet-limitations.png" alt="ControlNet limitations (page 57 top)">

- add conditioning control to a pretrained diffusion model without destroying its generative ability
  - conditions: edge, depth, pose, segmentation, scribble, etc.

**Core idea**

- copy the U-Net and attach a trainable control branch
- keep the original pretrained U-Net frozen (or mostly frozen)
- inject control features into the main U-Net via residual additions

**How it works (high-level)**

- control input $c$ is encoded into multi-scale features
- at each U-Net block, add control residuals to the main activations
- **"zero-conv"** (initialized near zero) makes training stable
  - starts from "no control effect" and gradually learns control strength

**Benefits**

- strong controllability with minimal degradation of text alignment and realism
- works as a plug-in for many conditions

**Limitations**

- extra compute and memory (another branch)
- needs paired data (image + condition) or a condition extraction pipeline

## T2I Adapter

<img class="note-img" src="/images/archives/notes/12-conditioning-and-distillation-p57-t2i-adapter.png" alt="T2I Adapter">

- lightweight conditioning adapter for pretrained text-to-image diffusion
  - similar use cases: edges, depth, pose, segmentation, style hints

**Core idea**

- add a small adapter network that encodes the condition into feature maps
- inject those features into the pretrained U-Net (often via residual connections)
- typically fewer parameters than ControlNet
- pretrained diffusion backbone stays frozen

**How it works (high-level)**

- condition encoder produces multi-scale representations
- features are fused into U-Net blocks (shallow additions)
- train only the adapter (and sometimes small projection layers)

**Benefits**

- parameter-efficient, faster to train, lower memory overhead
- easier to deploy when compute is tight

**Limitations**

- usually weaker controllability than full ControlNet
- capacity may be insufficient for complex spatial constraints

