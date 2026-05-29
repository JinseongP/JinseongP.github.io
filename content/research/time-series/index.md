---
title: "Time Series Analysis"
icon: "📈"
weight: 4
summary: "Advancing time series forecasting and classification with modern deep learning, including self-attention analysis, sharpness-aware training, and data-driven anomaly detection."
key_papers:
  - title: "Are Self-Attentions Effective for Time Series Forecasting?"
    venue: "NeurIPS 2024"
    url: "https://proceedings.neurips.cc/paper_files/paper/2024/hash/cf66f995883298c4db2f0dcba28fb211-Abstract-Conference.html"
  - title: "Fast sharpness-aware training for periodic time series classification and forecasting"
    venue: "Applied Soft Computing 2023"
    url: "https://www.sciencedirect.com/science/article/pii/S1568494623004854"
  - title: "Differentially private upsampling for enhanced anomaly detection in imbalanced data"
    venue: "Engineering Applications of AI 2026"
    url: "https://www.sciencedirect.com/science/article/abs/pii/S0952197625031550"
---

## Overview

Time series data is ubiquitous in finance, healthcare, and engineering. Our research develops principled deep learning methods tailored to the unique structure of sequential data.

## Key Directions

**Self-Attention for Time Series**: We critically evaluate whether Transformer-based self-attention is truly beneficial for time series forecasting, challenging prevailing assumptions and proposing efficient alternatives (CATS, NeurIPS 2024).

**Sharpness-Aware Training**: We adapt sharpness-aware minimization (SAM) to exploit the periodic structure of time series, achieving faster convergence and better generalization in classification and forecasting tasks (Applied Soft Computing 2023).

**Anomaly Detection**: We combine differential privacy techniques with upsampling strategies to improve anomaly detection in highly imbalanced industrial datasets (Engineering Applications of AI 2026).

**Diffusion-based Generation**: We develop TimeBridge, a diffusion bridge model that uses learned priors for controllable and realistic time series generation (KDD 2026).
