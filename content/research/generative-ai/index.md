---
title: "Generative AI & Diffusion Models"
icon: "🎨"
weight: 2
summary: "Studying the theory and applications of diffusion models, including time series generation, fairness-aware generation, machine unlearning, and financial modeling."
key_papers:
  - title: "TimeBridge: Better Diffusion Prior Design with Bridge Models for Time Series Generation"
    venue: "KDD 2026"
    url: "https://arxiv.org/abs/2408.06672"
  - title: "Fair Sampling in Diffusion Models through Switching Mechanism"
    venue: "AAAI 2024"
    url: "https://arxiv.org/abs/2401.03140"
  - title: "Co-occurring Associated REtained concepts in Diffusion Unlearning"
    venue: "ICLR 2026"
    url: "https://openreview.net/forum?id=Ryc7jKP6H9"
  - title: "Modeling Asset Price Process with Generative Diffusion Models"
    venue: "Computational Economics 2024"
    url: "https://link.springer.com/article/10.1007/s10614-024-10668-4"
---

## Overview

Diffusion models have emerged as the dominant paradigm for generative AI. Our research both advances diffusion model capabilities and applies them to novel domains, while ensuring they remain fair, safe, and interpretable.

## Key Directions

**Time Series Generation**: We develop diffusion bridge models that incorporate structural priors (periodicity, trends) for realistic time series synthesis, with applications to finance and healthcare (TimeBridge, KDD 2026).

**Fairness in Generation**: We propose attribute-switching mechanisms to ensure demographic fairness in diffusion model outputs without retraining from scratch (AAAI 2024).

**Machine Unlearning**: We study how to remove specific concepts from pretrained diffusion models while preserving associated knowledge, addressing the challenge of correlated concept removal (ICLR 2026).

**Financial Modeling**: We apply generative diffusion models to asset price simulation, improving volatility forecasting for quantitative finance (Computational Economics 2024).
