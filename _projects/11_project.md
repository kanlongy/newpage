---
layout: page
title: Rank-Supervised SKU Fingerprints for Low-Latency Generative Visual Search
description: Production-friendly fashion retrieval with unified image-text embedding and diffusion augmentation
img: assets/img/sku-overview.png
importance: 1
category: graduate
github: https://github.com/10623-GenAI-Final-Project/Rank-Supervised_SKU_Fingerprints_for_Low-Latency_Generative_Visual_Search.git
pdf: https://drive.google.com/file/d/1cjRfmpD0xdu2eBrMjgjWKKCU_rlyV3wr/view?usp=sharing
---

## Overview

**Period:** Oct. 2025 - Dec. 2025  
**Institution:** Carnegie Mellon University  
**Location:** Pittsburgh, USA  
**Type:** Course Project

**Keywords:** Vision-Language Retrieval, CLIP, Diffusion, VLA

[📄 Project Report (PDF)](https://drive.google.com/file/d/1cjRfmpD0xdu2eBrMjgjWKKCU_rlyV3wr/view?usp=sharing) | [🖼️ Poster](https://drive.google.com/file/d/16_D8M1IzjT8dhAzlyTOtF0jUp9_4hBpW/view?usp=sharing) | [💻 GitHub Repository](https://github.com/10623-GenAI-Final-Project/Rank-Supervised_SKU_Fingerprints_for_Low-Latency_Generative_Visual_Search.git)

---

## Introduction

Visual search in e-commerce requires balancing retrieval accuracy with production constraints. Existing methods either struggle with viewpoint variations or incur high inference costs. This project presents a **production-friendly SKU-per-vector retrieval system** that achieves robust fashion search on DeepFashion2 by:

1. Unifying image and text queries into a single embedding space
2. Leveraging offline Stable Diffusion augmentation for viewpoint robustness
3. Maintaining low-latency inference suitable for real-world deployment

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/sku-overview.png" title="System Overview" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    System overview: Rank-Supervised SKU Fingerprints for unified multi-modal fashion retrieval
</div>

---

## Methods

### 1. Unified Embedding Architecture

**Base Encoder:**
- Fine-tuned **CLIP ViT-B/16** as the backbone
- Shared embedding space for both image and text modalities
- SKU-per-vector indexing for efficient retrieval

**Rank-Supervised Learning:**
- Contrastive loss with hard negative mining
- Ranking supervision to preserve relative similarity ordering
- Multi-positive sampling for intra-SKU variations

### 2. Generative Data Augmentation

**Stable Diffusion v1.5 img2img Pipeline:**
- Generate multi-view variants of catalog images offline
- Control viewpoint, pose, and background variations
- Zero additional cost at inference time

**LoRA Fine-tuning:**
- Low-Rank Adaptation for fashion domain specialization
- Preserve garment identity while varying viewpoint
- Efficient fine-tuning with minimal parameters

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/sku-diffusion-augmentation.png" title="Diffusion Augmentation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Multi-view augmentation using Stable Diffusion img2img + LoRA: generating diverse viewpoints while preserving garment identity
</div>

### 3. Training Strategy

**Stage 1: Pre-training**
- Initialize from CLIP pre-trained weights
- Warm-up on large-scale fashion image-text pairs

**Stage 2: Fine-tuning with Augmented Data**
- Incorporate Stable Diffusion generated views
- Rank-supervised contrastive learning
- Hard negative mining across SKUs

**Stage 3: Index Construction**
- Compute SKU fingerprints (mean pooling of all views)
- Build FAISS index for approximate nearest neighbor search
- Single vector per SKU for production efficiency

---

## Results

### Image-to-SKU Retrieval

We evaluated image-based retrieval on DeepFashion2:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/sku-image-retrieval.png" title="Image-to-SKU Results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Image-to-SKU retrieval results: comparison with baseline methods
</div>

### Text-to-SKU Retrieval

Our unified embedding space enables seamless text-to-image retrieval:


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/sku-text-retrieval.png" title="Text-to-SKU Results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Text-to-SKU retrieval results: natural language queries successfully retrieve relevant fashion items
</div>

### Key Findings

**1. Diffusion augmentation significantly improves viewpoint robustness**
- +8.4% Recall@1 compared to training without augmentation
- Generated views cover unseen angles and poses

**2. Rank supervision outperforms standard contrastive learning**
- +3.2% average recall improvement
- Better preserves fine-grained similarity relationships

**3. Production-friendly latency**
- Only 9.1ms per query (similar to vanilla CLIP)
- 5× faster than composed image retrieval methods

---

## Qualitative Results

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/sku-single-retrieval.png" title="Single Retrieval Example" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Single query retrieval example: query image (left) and top-K retrieved SKUs ranked by similarity
</div>

---

## Discussion

### Production Considerations

- **Single embedding per SKU**: Minimizes index size and memory footprint
- **Offline augmentation**: All diffusion computation done during training
- **Scalable indexing**: Compatible with FAISS, ScaNN, and other ANN libraries

### Limitations

- **Augmentation quality**: Diffusion may occasionally alter garment details
- **Text encoder**: CLIP text encoder may miss fashion-specific terminology
- **Domain gap**: Performance may degrade on out-of-distribution styles

---

## Conclusion

We present a production-friendly fashion retrieval system that achieves state-of-the-art performance on DeepFashion2 while maintaining low inference latency. By combining rank-supervised learning with offline Stable Diffusion augmentation, our method improves robustness to viewpoint variations without increasing deployment costs. The unified embedding space enables flexible multi-modal queries, making it suitable for real-world e-commerce applications.
