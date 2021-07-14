---
title: Fawkes: Protecting Privacy against Unauthorized Deep Learning Models 논문 요약
excerpt: USENIX 2020에 발표된 Shawn Shan외의 'Fawkes: Protecting Privacy against Unauthorized Deep Learning Models' 논문을 요약한 글입니다.
categories:
  - papers
tags:
  - Privacy
  - Adversarial attack
---
# Fawkes: Protecting Privacy against Unauthorized Deep Learning Models

Author: Shawn Shan, Emily Wenger, Jiayun Zhang, Huiying Li, Haitao Zheng, and
Ben Y. Zhao
Publish: USENIX
Status: Finished
Sub-Topic: Privacy
Topic: Adv
Year: 2020

## Introduction

### Introduction

1. [Clearview.ai](http://clearview.ai/) (facial recognition search engine → prevent crime) 와 같은 Privacy 침해 문제가 발생함.
2. inoculate their images against unauthorized facial recognition models
3. model의 train (transfer, from scratch)에 상관 없이 95% 이상의 방어 성능

### Background

1. Poisoning attack, Evasion attack
    - Poisoning attack과 Evasion attack에 따라 가정하는 상황이 다름
    - Poisoning attack: train dataset에 추가적으로 adding a set of samples S and associated labels $L_s$ .
    - Evasion attack: adversarial attack과 같이 train된 모델을 attack해서 제대로 classify 못하게
2. Transfer Learning
3. Adversarial Example with perturbation → "cloaks" 라고 표현함
4. Real world problem에서는 내가 아니라 다른 사람의 이미지에 올라온 내 사진까지 cloak 할 순 없음. 다만 이를 통해 "Right to be forgotten"

## Methodology

### Notation

![Untitled.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled.png){:.center-image}

### Computing Cloak Perturbations

1. Loss
    - $\delta$(DSSIM)을 일정 $\rho$ 이내로 유지하면서 feature space의 거리를 줄여주는 $\delta$.

![Untitled%201.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%201.png){:.center-image}

![Untitled%202.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%202.png){:.center-image}

    - targeted attack을 진행하는데 target은 photos to be shared online $X_u$  와 거리가 가장 먼 centroid를 가진 target을 고름. (dissimilar from the feature representations of all images)

![Untitled%203.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%203.png){:.center-image}

![Untitled%204.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%204.png){:.center-image}

2. DSSIM $\delta$
- PERCEPTUAL ADVERSARIAL ROBUSTNESS: DEFENSE AGAINST UNSEEN THREAT MODELS 에서 사용한 것처럼 Vision 분야에서 쓰는 사람의 눈에 보이는 차이 사용 (LPIPS → PAT)

![Untitled%205.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%205.png){:.center-image}

![Untitled%206.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%206.png){:.center-image}

### Scenario

1. 내가 올린 이미지는 기존 모델(uncloaked image 모델)에 대해서 evasion attack
2. 내가 올린 cloaked image를 가지고 tracker가 train하면 poisoning attack처럼 작용

![Untitled%207.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%207.png){:.center-image}

## Result

### Transfer attack

1. teacher (WebFace, VGG / Inception, DenseNet ) → Student (PubFig, FaceScrub)

![Untitled%208.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%208.png){:.center-image}

![Untitled%209.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%209.png){:.center-image}

2. Student가 teacher와 같은 모델(feature extractor)를 쓰냐, 다른 모델을 쓰냐에 따라 시나리오 다름.

### Same feature extractors

1. cloak이 잘 된다 (target을 향해서)
2. label 숫자가 적으면 feature space와 label간의 association이 커져서 label이 많을수록 cloak가 쉽다.

![Untitled%2010.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%2010.png){:.center-image}

### Different feature extractors

1. AT를 통해서 robust feature extractor를 학습하자.
2. PGD algorithm for 100 steps using a step size of 0.01

![Untitled%2011.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%2011.png){:.center-image}

### In the wild

1. 기존의 모델들에 대해서도 cloak 잘한다

![Untitled%2012.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%2012.png){:.center-image}

## Additional Result

### Sybil attack

1. 시빌공격(sybil attack)이란 공격자가 실제로는 한 명이면서 마치 여러 명인 것처럼 속이는 방식으로 네트워크상의 여러 노드를 제어함으로써, 의사결정에 좋지 않은 영향을 미치는 공격을 말한다. 익명성을 기반으로 운영되는 인터넷 백과사전인 위키백과에서 실제로는 한 명의 사용자가 여러 개의 다중계정을 만들고 악의적으로 사용함으로써 의사결정에 혼란을 주는 행위와 유사하다.
2. secondary identity → 뭐 이런 방식으로 cloak을 사용할 수 있다.

![Untitled%2013.png]({{site.url}}/assets/images/2021-7-14-Fawkes Protecting Privacy against Unauthorized Deep Learning Models 논문 요약/Untitled%2013.png){:.center-image}

## Conclusion

1. first face recognition privacy system
2. 오히려 범죄자들이 이걸 사용하면 범죄자 얼굴 추적 불가능해질수도?
