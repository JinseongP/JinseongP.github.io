---
title: Why do adversarial attacks transfer? explaining transferability of evasion and poisoning attacks 논문 요약
excerpt: USENIX 2019에 발표된 A Demontis외의 'Why do adversarial attacks transfer? explaining transferability of evasion and poisoning attacks' 논문을 요약한 글입니다.
categories:
  - papers
tags:
  - Adversarial attack
---
# Why Do Adversarial Attacks Transfer? Explaining Transferability of Evasion and Poisoning Attacks

Author: Ambra Demontis, Marco Melis, Maura Pintor, Matthew Jagielski, Battista Biggio, Alina Oprea, Cristina Nita-Rotaru, Fabio Roli
Publish: USENIX
Status: Finished
Sub-Topic: Transfer
Topic: Adv
Year: 2019

## Summary & Idea

1. adversarial attack을 loss landscape 관점에서 본 것
2. 왜 transfer가 잘되는가

## Introduction

### Introduction

1. adversarial attack 중 evasion attack과 poisoning attack 모두 transfer(surrogate → target)가 가능하다는 것이 연구됨.
2. 하지만 why, when에 대한 연구가 없음.
3. 주로 gradient based attack에 대해 optimization 관점에서 살펴보겠다.
4. 중요한 세 가지를 발견함
    - intrinsic adversarial vulnerability of the target model → complexity of the target model → target model is robust (reducing size of the input gradient) → well transfered
    - complexity of the surrogate model → complexity of the surrogate model → low is well transfered
    - alighnment of surrogate and target model → alignment of the gradients → high alignment is well transfered
5. complexity of surrogate and target model

![Untitled.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled.png){:.center-image}

## Methodology

### Notation

1. Attacker: white box and black box setting이 존재, query 해 볼 수 없음.
2. hat을 붙인 애들이 surrogate classifier

![Untitled%201.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%201.png){:.center-image}

3. target classifier

![Untitled%202.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%202.png){:.center-image}

### Attacks

1. gradient-based attack

![Untitled%203.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%203.png){:.center-image}

2. Evasion attack and poisoning attack (using KKT)

![Untitled%204.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%204.png){:.center-image}

![Untitled%205.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%205.png){:.center-image}

3. SVM, Logistic Regression 등 다양한 Poisoning gradient를 구하는 방법에 대해서 설명

### Transferability Measure

1. define its transferability as the loss attained by the target classifier f (parameterized by w) on that point  → loss gradient가 클수록 perturbation 되었을 때의 loss가 커진다 → attack을 잘 당한다

![Untitled%206.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%206.png){:.center-image}

2. 위의 식이 hold for sufficiently-small input perturbations + small curvature(strongly regularized)이면 large input perturbation에 대해서도 성립

![Untitled%207.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%207.png){:.center-image}

3. $\epsilon-$sized ball → dual norm

![Untitled%208.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%208.png){:.center-image}

4. ex) $\ell_2$

![Untitled%209.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%209.png){:.center-image}

### Intriguing Connections and Transferability Metrics

1. size of input gradients (target) → size of input gradient가 클수록 attack을 잘 당함

![Untitled%2010.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%2010.png){:.center-image}

    - strongly regularized (weight decay...) → smaller input grads → smoother functions and robust to attacks
    - 대신 이걸 가지고 NN과 SVM (다른 모델)을 비교할 수는 없다.

![Untitled%2011.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%2011.png){:.center-image}

2. Gradient Alignment
    - ${\Delta \ell }\over{||\Delta_X \ell }||_2$ in $\ell_2$ norm

![Untitled%2012.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%2012.png){:.center-image}

    - cosine of the angle between the gradient of surrogate and that of the target classifier
    - 이전 연구에서는 empirical 하게만 말했는데 자기들이 수식으로 novel finding했다.
3. Variability of the Loss landscape
    - surrogate model의 loss가 local optima가 많으면 simply resampling같은 것만으로도 optimization 문제가 생김
    - surrogate model의 loss가 variability가 작아야 한다.

![Untitled%2013.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%2013.png){:.center-image}

![Untitled%2014.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%2014.png){:.center-image}

## Experiment

### Experiment

1. epsilon이 커지면 error가 커진다. 같은 모델에서 Low complexity (점선)이 High complexity (실선) 보다 낮다.

![Untitled%2015.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%2015.png){:.center-image}

2. input gradient size가 작을수록 error가 낮다.

    loss landscape의 variability가 낮을수록 transfer를 잘한다.

    gradient alignment가 높을수록 correlation이 크다.(Pearson, Kendall)

![Untitled%2016.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%2016.png){:.center-image}

3.  surrogate 모델을 구성할 때 train data의 20%만 써도 black box에서 전체 data와 비슷한 수준의 attack rate를 얻을 수 있었다. → 쿼리도 필요 없고 비슷한 데이터만 있으면 transfer 가능

### Experiment (Poisoning)

- 자세히 읽진 못했지만 하고 싶은 말은 Evasion이랑 큰 차이는 없는듯.
- 저자가 요약한 글

![Untitled%2017.png]({{site.url}}/assets/images/2021-7-14-Why do adversarial attacks transfer explaining transferability of evasion and poisoning attacks 논문 요약/Untitled%2017.png){:.center-image}
