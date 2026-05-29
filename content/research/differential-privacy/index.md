---
title: "Differential Privacy & Machine Learning"
icon: "🔒"
weight: 1
summary: "Developing privacy-preserving machine learning algorithms with rigorous differential privacy (DP) guarantees, including DP-SGD optimization, private SVMs, and synthetic data generation."
key_papers:
  - title: "Differentially Private Sharpness-Aware Training"
    venue: "ICML 2023"
    url: "https://proceedings.mlr.press/v202/park23g.html"
  - title: "Multi-Class Support Vector Machine with Differential Privacy"
    venue: "NeurIPS 2025"
    url: "https://neurips.cc/virtual/2025/poster/118680"
  - title: "In-distribution Public Data Synthesis with Diffusion Models for Differentially Private Image Classification"
    venue: "CVPR 2024"
    url: "https://openaccess.thecvf.com/content/CVPR2024/html/Park_In-distribution_Public_Data_Synthesis_with_Diffusion_Models_for_Differentially_Private_CVPR_2024_paper.html"
  - title: "Efficient differentially private kernel support vector classifier for multi-class classification"
    venue: "Information Sciences 2023"
    url: "https://www.sciencedirect.com/science/article/pii/S0020025522011951"
---

## Overview

Differential Privacy (DP) provides a rigorous mathematical framework for protecting individual data points in machine learning pipelines. Our research addresses fundamental challenges in making deep learning models both accurate and privacy-preserving.

## Key Directions

**DP-SGD Optimization**: We study how sharpness-aware minimization and other advanced optimizers interact with DP-SGD, finding that flat minima improve generalization under privacy constraints (DPSAT, ICML 2023).

**Private SVMs**: We design computationally efficient DP kernel SVMs that scale to multi-class settings without sacrificing utility (Information Sciences 2023; NeurIPS 2025).

**Synthetic Data for DP Training**: We leverage diffusion models to generate in-distribution public data, enabling stronger DP-SGD training without additional private data collection (CVPR 2024).

**Privacy in LLMs & RAG**: We investigate membership inference attacks on retrieval-augmented generation systems and develop defenses (EMNLP Findings 2025).
