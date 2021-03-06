---
title: Perceptual adversarial robustness; Defense against unseen threat models 논문 요약
excerpt: ICLR 2021에 발표된 Cassidy Laidlaw외의 'Perceptual adversarial robustness; Defense against unseen threat models' 논문을 요약한 글입니다.
categories:
  - papers
tags:
  - Adversarial attack
---

# [PAT] PERCEPTUAL ADVERSARIAL ROBUSTNESS: DEFENSE AGAINST UNSEEN THREAT MODELS

Author: Cassidy Laidlaw, Sahil Singla, Soheil Feizi
Publish: ICLR
Status: Finished
Sub-Topic: Adversarial Training
Topic: Adv
Year: 2021

2021 ICLR,  Cassidy Laidlaw, Sahil Singla, Soheil Feizi

## Introduction

### threat models

- exisitng threat models: $L_2, L_\infty,$ StAdv (Spatially Transformed Adversarial Examples), ReColor ...
- adversary will only attempt attacks within that threat model
- do not usually generalize well to unforeseen threat models
- susceptible to other imperceptible adversarial examples that are not contained in any of the constituent threat models

    training against the set of all imperceptible adversarial examples

### perceptual adversarial threat model

1. unrestricted adversarial threat model (Brown et al., 2018): any adversarial example that is labeled as one class by a classiﬁer but a different class by humans (label이 중요)
2. perceptual adversarial threat model: all perturbations of natural images that are imperceptible to a human (사람이 인지하지 못하는 수준의 모든 perturb)

![Untitled.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled.png){:.center-image}

- perceptual distance $d^*(\mathbf{x,\tilde{x}})\le \epsilon^*$→ neural perceptual distance (approximation)
    - using surrogate perceptual distances in computer vision literatures
    - **Learned Perceptual Image Patch Similarity (LPIPS)**, Zhang et al. (2018) 인용수 1100회
    - **comparing the internal activations** of a convolutional neural network when two different images are passed through provides a measure, that correlates well with **human perception**.

neural perceptual threat model (NPTM) using LPIPS

## Related Work

### Perceptual similarity

- $L_2$, Signal-to-Noise-Ratio(PRNR): basic similarity measures for images

    → disagree with human vision

- SSIM, MS-SSIM, ... LPIPS
- 그 중에서 LPIPS가 가장 human vision이랑 비슷하고, unforseen threat models에 대해 가장 generalize well

## NEURAL PERCEPTUAL THREAT MODEL (NPTM)

### LPIPS distance

- $g:\mathcal{X\rightarrow Y}$ convolutional image classiﬁer network, images $\mathbf{x}\in \mathcal{X}$
- $g$ have $L$ layers, internal activations (outputs) of the l-th layer $g_l(\mathbf{x})$
- **normalizing** and then comparing the internal activations of convolutional neural networks correlates well with **human similarity judgements**. (Zhang et al., 2018)
- normalized **activations** (layer size and flattered to a singlevector)
$\phi(\mathbf{x}) \triangleq({(\hat{g_1}(\mathbf{x})\over \sqrt{w_1 h_1}},...,{(\hat{g_L}(\mathbf{x})\over \sqrt{w_L h_L}})$, $w$: width, $h$: height
- LPIPS distance $d(\mathbf{x_1,x_2})\triangleq \|\phi(\mathbf{x_1})-\phi(\mathbf{x_2})\|_2$
- perceptual adversarial example
$f(\tilde{\mathbf{x}})\neq y, d(\mathbf{x,\tilde{x}})=\|\phi(\mathbf{x})-\phi(\mathbf{\tilde{x}})\|_2 \leq \epsilon$

## PERCEPTUAL ADVERSARIAL ATTACKS

![Untitled%201.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%201.png){:.center-image}

### Margin loss

- $L(f(\mathbf{x}),y)\triangleq max_{i\neq y} (z_i(\mathbf{x})-z_y(\mathbf{x}))$ CW margin loss
- $max_{\tilde{x}}L(f(\tilde{\mathbf{x}}),y) \text{ s.t  } d(\mathbf{x},\tilde{\mathbf{x}})=\|\phi(\mathbf{x})-\phi{\mathbf{(\tilde{x})}}\|_2\leq\epsilon$  (3)
- classifier network $f(\cdot)$, LPIPS network $g(\cdot)$ are fixed.
- 둘다 LPIPS → self-bounded attack, 다른 network를 LPIPS → externally-bounded attack

### Perceptual Projected Gradient Descent (PPGD) ← PGD

- desire to find a step δ to **maximize L(f(x+δ), y) such that d(x+δ, x) =** $\|\phi(\mathbf{x}+\delta)-\phi(\mathbf{x})\|_2 \leq\eta$where η is the step size.
- $\hat{f}(\mathbf{x}):=L(f(\mathbf{x}),y), J:\text{Jacobian of }\phi(\cdot), \nabla\hat{f}:\text{gradient of } \hat{f} \text{ at } \mathbf{x}$
- $max_\delta \hat{f}(\mathbf{x})+(\nabla\hat{f})^T\delta \text{     s.t }\|J\delta\|_2\leq \eta.$ (first order Taylor's approximation of 3)

- lemma1: Let $J^+$ pseudoinverse of $J$.

![Untitled%202.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%202.png){:.center-image}

- 하지만 이 방법도 $(J^TJ)^{-1}$와 $J^T$를 구하는 데 오래 걸림 → conjugate gradient method (Appendix A.1)
- non-convexity feasible set → Newton's method
- Algorithm

![Untitled%203.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%203.png){:.center-image}

### Lagrangian Perceptual Attack (LPA) ← CW attack

![Untitled%204.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%204.png){:.center-image}

- $\|\phi(\mathbf{x}+\delta)-\phi(\mathbf{x})\|_2 \leq \epsilon$ → 0
- $\|\phi(\mathbf{x}+\delta)-\phi(\mathbf{x})\|_2 \gt \epsilon$ → **increases linearly by the LPIPS distance** from the original input x
- Algorithm

![Untitled%205.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%205.png){:.center-image}

## PERCEPTUAL ADVERSARIAL TRAINING (PAT)

### Perceptual Projected Gradient Descent (PPGD) ← PGD

- minimize the worst-case loss within a neighborhood of each training point x.
→ bounded by the LPIPS distance
- attack에는 inexpensive한 Fast-LPA (does not search over values of $\lambda$ → projection 생략)

![Untitled%206.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%206.png){:.center-image}

- Algorithm

![Untitled%207.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%207.png){:.center-image}

## PERCEPTUAL EVALUATION

### human comparisons of similarity

- Amazon Mechanical Turk (AMT)
- ImageNet-100, adversarial perturbation of the natural image
- 두 이미지 중 하나가 randoly하게 1초동안 보여주고 그 다음 이미지 1초동안
- 두 이미지를 same or different하다고 믿는가? test
- **“different”** as the perceptibility of the **attack** → only **4.1%** of these were annotated as “different.”
- AlexNet-based LPIPS correlates best with human perception > ResNet 50

![Untitled%208.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%208.png){:.center-image}

our  proposed  PPGD  and  LPA  attacks  reduce  a  PAT-trained  classiﬁer  to  even lower  accuracies  (8.2% for PPGD and 0% for LPA), making  it  the  strongest  attack  studied.

## EXPERIMENTS

### settings

- union accuracy: (L∞ , L2 , StAdv, ReColorAdv, and JPEG for ImageNet-100)
- unseen mean accuracy: mean of the accuracies against all the threat models not trained against; → generalization

### result

- experimental results

![Untitled%209.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%209.png){:.center-image}

![Untitled%2010.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%2010.png){:.center-image}

- multiple-attack training methods 한 거보다 잘함.

### why?

- training would require using a huge training bound, resulting in poor performance

![Untitled%2011.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%2011.png){:.center-image}

1) LPIPS distance correlates with human perception
2) LPIPS distance를 활용한 attack, defense
3) 이게 generalization이 잘 되는 defense더라
4) 그 이유는 distance가 비슷하게 나와서?

[추가]

hamming distance: 블록 부호 이론에서, 해밍 거리는 곱집합 위에 정의되는 거리 함수이다. 대략, 같은 길이의 두 문자열에서, 같은 위치에서 서로 다른 기호들이 몇 개인지를 센다.

![Untitled%2012.png]({{site.url}}/assets/images/2021-7-14-Perceptual adversarial robustness; Defense against unseen threat models 논문 요약/Untitled%2012.png){:.center-image}

Relu → restricted to a sufficiently small neighborhood of x is a linear function (local linear)

local stability ~ Hamming distances
