---
layout: page
title: DDPM-AFHQ - Denoising Diffusion Probabilistic Models
description: End-to-end diffusion model implementation on AFHQ dataset
img: assets/img/ddpm.gif
importance: 3
category: graduate
github: https://github.com/kanlongy/DDPM_AFHQ.git
---

## Overview

**Period:** Jun. 2025 - Aug. 2025  
**Institution:** Carnegie Mellon University  
**Location:** Pittsburgh, USA  
**Type:** Personal Project

---

## Introduction

This project implements a full **Denoising Diffusion Probabilistic Model (DDPM)** using the AFHQ dataset (cats subset). The work follows the modern diffusion modeling pipeline, covering the **forward process**, **reverse denoising process**, **cosine noise schedule**, **U-Net denoiser**, **training with L1 noise prediction loss**, and **visualization of both forward and backward diffusion**. All training experiments were completed on an A100 GPU.

**Keywords:** Generative Models, Diffusion Models, Computer Vision, U-Net Implementation

[💻 GitHub Repository](https://github.com/kanlongy/DDPM_AFHQ.git)

---

## Diffusion Forward & Reverse Processes

The forward process gradually adds Gaussian noise:

$$
x_t = \sqrt{\bar{\alpha}_t} x_0 + \sqrt{1-\bar{\alpha}_t}\epsilon, \qquad \epsilon \sim \mathcal{N}(0,I)
$$

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ddpm_forward_f.png" title="Forward diffusion formulation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The reverse process uses the U-Net to predict $$\epsilon_\theta(x_t,t)$$ and reconstruct $$x_0$$ through the posterior mean:

$$
\tilde{\mu}_t =
\frac{\sqrt{\alpha_t}(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t} x_t
+
\frac{\sqrt{\bar{\alpha}_{t-1}}(1-\alpha_t)}{1-\bar{\alpha}_t} \hat{x}_0
$$

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ddpm_backward_f.png" title="Reverse diffusion formulation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

### Forward & Reverse Diffusion Visualization

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ddpm_forward.png" title="Forward diffusion process" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ddpm_backward.png" title="Reverse diffusion process" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Forward diffusion process (noise increases gradually). Right: Reverse diffusion process (model reconstructs from noise).
</div>

---

## U-Net Noise Predictor

A lightweight U-Net was implemented with:

- Downsampling/upsampling blocks
- Skip connections
- GroupNorm + SiLU
- Time embedding injected into each residual block
- Middle-layer attention at low resolution

This network predicts the added noise $$\epsilon_\theta(x_t,t)$$, enabling reconstruction of $$x_0$$.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ddpm_unet.jpeg" title="U-Net architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    U-Net architecture for noise prediction
</div>

---

## Training Pipeline & Configuration

**Objective:**

$$
\mathcal{L} = \| \epsilon - \epsilon_\theta(x_t,t) \|_1
$$

**Setup:**

- Dataset: AFHQ (cats)
- Timesteps: T = 50
- Optimizer: AdamW (1e-3)
- Batch size: 32
- Cosine noise schedule (Nichol & Dhariwal 2021)
- Hardware: Google Colab A100

---

## Results

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ddpm_training_loss.png" title="Training loss curve" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Training loss curve showing convergence over 10k steps
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ddpm_fid.png" title="FID progression" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    FID (Fréchet Inception Distance) progression over training steps
</div>

---

## My Role & Contributions

As the sole developer, I:

- Implemented the full forward diffusion process
- Implemented reverse process sampling (`p_sample`, `p_sample_loop`, `sample`)
- Built the U-Net noise prediction model
- Engineered full training pipeline and L1 noise prediction loss
- Logged FID and produced visualizations (forward, backward, final samples)
- Completed full 10k-step training and evaluation
- Extracted and documented all results shown above

This project refined my understanding of variational inference, generative modeling, and the finer dynamics of diffusion processes.
