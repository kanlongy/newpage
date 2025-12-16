---
layout: page
title: Textual Attributes for Speech Understanding with LLMs
description: Training-free pipeline converting speech to textual attributes for LLM reasoning
img: assets/img/speech-pipeline.png
importance: 1
category: graduate
github: https://github.com/cyhuang-tw/anlp-hw4.git
pdf: https://drive.google.com/file/d/1v_WHnBDodMMk-bFdZXM8Xa7zoguxgeqE/view?usp=sharing
---

## Overview

**Period:** Oct. 2025 - Dec. 2025  
**Institution:** Carnegie Mellon University  
**Location:** Pittsburgh, USA  
**Type:** Course Project

**Keywords:** LLM, Speech Processing, Multimodal Reasoning

[📄 Project Report (PDF)](https://drive.google.com/file/d/1v_WHnBDodMMk-bFdZXM8Xa7zoguxgeqE/view?usp=sharing) | [🖼️ Poster](https://drive.google.com/file/d/1w_sCh7YSvqxipZbSQXKvNwI8eisOF6l7/view?usp=sharing) | [💻 GitHub Repository](https://github.com/cyhuang-tw/anlp-hw4.git)

---

## Introduction

Current speech-language models (SLMs) face significant challenges: they require expensive end-to-end training on large-scale speech-text paired data, and their speech encoders often fail to capture fine-grained acoustic features like prosody and speaker characteristics. This project presents a **training-free pipeline** that converts speech into rich textual representations—semantic, prosodic, and speaker attributes—enabling any text-based LLM to perform speech understanding tasks without additional training.

---

## Methods

Our approach extracts three complementary types of textual attributes from speech signals:

### 1. Semantic Attributes (ASR Transcription)

- Utilize **Whisper** for automatic speech recognition
- Generate accurate text transcriptions as the semantic foundation
- Preserve linguistic content for downstream LLM processing

### 2. Prosodic Attributes

- Extract **pitch contours** (fundamental frequency patterns)
- Analyze **energy/intensity** variations
- Compute **speaking rate** and rhythm features
- Quantize continuous prosodic features into discrete textual tokens

### 3. Speaker Attributes

- Leverage pre-trained speaker embedding models
- Extract **speaker identity** descriptors
- Capture **voice characteristics** (age, gender, emotion indicators)
- Convert speaker embeddings to textual descriptions

### 4. LLM Reasoning

- Concatenate all textual attributes into structured prompts
- Feed formatted text to off-the-shelf LLMs (e.g., GPT-4, LLaMA)
- Enable zero-shot speech understanding without speech-specific training

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/speech-pipeline.png" title="Pipeline Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Complete pipeline: from speech waveform to LLM-readable textual attributes
</div>

---

## Dataset: Dynamic-SUPERB

We evaluate on **Dynamic-SUPERB**, a comprehensive benchmark covering diverse speech understanding tasks organized into a hierarchical taxonomy:

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/speech-task-taxonomy.png" title="Task Taxonomy" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Task taxonomy in Dynamic-SUPERB: hierarchical organization of speech understanding tasks
</div>

The benchmark includes:
- **Content**: Intent classification, keyword spotting, automatic speech recognition
- **Speaker**: Speaker identification, verification, age/gender recognition
- **Semantics**: Emotion recognition, sentiment analysis, sarcasm detection
- **Degradation**: Noise detection, enhancement quality assessment
- **Paralinguistics**: Accent classification, language identification

---

## Results

### Baseline Comparison

We evaluated our method against state-of-the-art speech-language models on Dynamic-SUPERB core tasks:


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/speech-baseline-results.png" title="Baseline Comparison" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Performance comparison with baseline methods on Dynamic-SUPERB
</div>

### Key Findings

**1. Training-free approach outperforms end-to-end models**
- No fine-tuning required on speech-text paired data
- Achieves 4.7% average improvement over Qwen2-Audio

**2. Prosodic attributes are crucial for emotion tasks**
- Pitch contours strongly correlate with emotional expression

**3. Speaker attributes enhance identification tasks**
- Voice characteristics provide discriminative cues

---

## Ablation Studies

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/speech-ablation.png" title="Ablation Study" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Ablation study: contribution of each attribute type to overall performance
</div>

The results demonstrate that:
- **Semantic attributes** form the essential foundation
- **Prosodic attributes** contribute significantly to acoustic understanding
- **Speaker attributes** provide complementary discriminative information
- The combination achieves synergistic improvements

---

## Discussion

### Advantages

- **Zero training cost**: Leverages existing pre-trained models
- **Modular design**: Easy to upgrade individual components
- **Interpretable**: Textual attributes are human-readable
- **Scalable**: Benefits from advances in both speech and language models

### Limitations

- **Cascading errors**: ASR mistakes propagate to downstream tasks
- **Information loss**: Quantization may lose fine-grained acoustic details
- **Latency**: Multiple model calls increase inference time

---

## Conclusion

We present a training-free pipeline that bridges the gap between speech and language understanding by converting acoustic signals into rich textual attributes. Our approach outperforms state-of-the-art speech-language models like Whisper-LLaMA and Qwen2-Audio on Dynamic-SUPERB benchmarks, demonstrating that careful attribute extraction can enable powerful LLMs to reason about speech without expensive end-to-end training.
