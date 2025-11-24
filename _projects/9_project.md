---
layout: page
title: Pittsburgh RAG - Retrieval-Augmented Generation System
description: Building a RAG system for Pittsburgh & CMU knowledge
img: assets/img/pittsburgh-rag-overview.png
importance: 1
category: graduate
github: https://github.com/kanlongy/Pittsbugrh-RAG.git
pdf: https://drive.google.com/file/d/1tRV8AUEn1Guqk6ctFeHWvKgqcFcuuWwg/view?usp=sharing
---

## Overview

**Period:** Sep. 2025 - Oct. 2025  
**Institution:** Carnegie Mellon University  
**Location:** Pittsburgh, USA  
**Type:** Course Project

**Keywords:** LLM, NLP, Information Retrieval, RAG

[📄 Project Report (PDF)](https://drive.google.com/file/d/1tRV8AUEn1Guqk6ctFeHWvKgqcFcuuWwg/view?usp=sharing) | [💻 GitHub Repository](https://github.com/kanlongy/Pittsbugrh-RAG.git)

---

## Introduction

Understanding local information—such as events, museums, sports, and campus resources—should be simple, but online data is often scattered across hundreds of pages. Our project addresses this gap by building a **Retrieval-Augmented Generation (RAG) system** capable of answering free-form questions about Pittsburgh and Carnegie Mellon University (CMU) with high accuracy.

The system improves access to local knowledge, supports new students and residents, and demonstrates how AI can organize large-scale heterogeneous information into clear, trustworthy answers.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/pittsburgh-rag-overview.png" title="System Overview" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    RAG Pipeline Architecture - from data collection to answer generation
</div>

---

## Methods

We designed a full RAG pipeline consisting of a **knowledge base**, **retrievers**, and a **reader (LLM)**. The overall system follows the three-stage workflow:

### 1. Data Collection & Knowledge Base Construction

- Scraped data from official Pittsburgh & CMU websites
- Processed **77 raw data files** (HTML, PDFs → JSON/TXT)
- Organized knowledge into **6 domains**: Events, Museums, Music & Culture, Food, Sports, General Info
- Produced thousands of chunked text documents

### 2. Test Set & Evaluation Dataset

- Created a **150-question QA test set**
- Measured annotation quality with **IAA (Krippendorff's $$\alpha$$ soft): 0.784**, confirming high annotation reliability

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/IAA-result.png" title="IAA Results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Inter-Annotator Agreement (IAA) and performance against the golden standard
</div>

### 3. Model Architecture

**Embedder:**

- `all-MiniLM-L6-v2` (efficient semantic encoder)
- `multi-qa-mpnet-base-dot-v1` (strong QA-optimized model)

**Retrievers:**

- **Sparse**: BM25
- **Dense**: FAISS cosine search
- **Hybrid retrieval**: normalized weighted fusion

**Re-ranker:**

- Cross-encoder: `BAAI/bge-reranker-v2-m3`

**Reader (Generator):**

- Gemma-2 9B IT (4-bit quantized)
- Generates final answers using retrieved context

---

## Results

Our experiments tested the influence of reader size, retriever type, top-k, and reranking.

### Key Findings

**1. RAG dramatically improves accuracy**

- No-RAG F1: 28.93%
- **Best RAG F1: 74.20%** ✨

**2. Hybrid Retrieval + Reranker achieves the best performance**

- Recall: 83.11%
- EM: 46.67%
- F1: 74.20%

**3. Reader size matters**

- Gemma-9B ≫ Gemma-2B in accuracy, completeness, and reasoning

**4. Top-K matters**

- k = 10 gives best F1 (more context)
- k = 5 sometimes gives better EM (more precise)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/pittsburgh-rag-results.png" title="Performance Comparison" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Performance comparison across different configurations
</div>

Overall, combining sparse & dense signals, then reranking, leads to the most reliable answers.

---

## Discussion

Our analysis highlights several insights about real-world RAG behavior:

- **RAG is most helpful for time-sensitive or niche information**, such as events, schedules, or location-specific facts
- **Pure LLMs cannot track local or updated knowledge**, while retrieval ensures factual grounding
- **Sparse retrieval surprisingly outperforms dense retrieval alone**, due to keyword-heavy city/event information
- **Hybrid + reranker provides a strong balance** between precision and coverage

---

## Conclusion

We successfully built a practical, robust RAG system that improves access to localized Pittsburgh and CMU knowledge. Through systematic evaluation, we show that combining **hybrid retrieval**, **strong readers**, and **cross-encoder reranking** yields the best performance.
