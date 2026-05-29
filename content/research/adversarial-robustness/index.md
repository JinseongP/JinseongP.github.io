---
title: "Adversarial Robustness"
icon: "🛡️"
weight: 3
summary: "Understanding and improving the robustness of deep learning models against adversarial attacks, with focus on certifiable defenses, robust generalization measures, and audio security."
key_papers:
  - title: "Fantastic Robustness Measures: The Secrets of Robust Generalization"
    venue: "NeurIPS 2023"
    url: "https://openreview.net/forum?id=AGVBqJuL0T"
  - title: "Towards Better Understanding of Training Certifiably Robust Models"
    venue: "NeurIPS 2021"
    url: "https://proceedings.neurips.cc/paper/2021/hash/07c5807d0d927dcd0980f86024e5208b-Abstract.html"
  - title: "Generating Transferable Adversarial Examples for Speech Classification"
    venue: "Pattern Recognition 2023"
    url: "https://www.sciencedirect.com/science/article/pii/S0031320322007658"
---

## Overview

Adversarial robustness is critical for deploying machine learning models in safety-critical applications. Our work spans both theoretical understanding and practical defenses, covering both visual and audio domains.

## Key Directions

**Robust Generalization**: We systematically study which robustness measures (sharpness, spectral norms, margin) best predict robust generalization gap, providing practical guidelines for training (NeurIPS 2023).

**Certifiable Robustness**: We analyze the loss landscape geometry in certified adversarial training, revealing why standard training fails and how landscape-aware methods improve certified accuracy (NeurIPS 2021).

**Audio Adversarial Attacks**: We study the transferability of adversarial examples in speech classification systems, demonstrating vulnerabilities in deployed speech recognition models (Pattern Recognition 2023).

**Jacobian Regularization**: We propose impurity-weighted implicit Jacobian regularization that improves generalization without explicit computation (ICML 2023).
