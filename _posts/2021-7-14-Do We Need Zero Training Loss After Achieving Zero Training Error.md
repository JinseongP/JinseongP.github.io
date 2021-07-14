---
title: Do We Need Zero Training Loss After Achieving Zero Training Error? 논문 요약
excerpt: ICML 2020에 발표된 Takashi Ishida외의 'Do We Need Zero Training Loss After Achieving Zero Training Error?' 를 요악한 글입니다.
categories:
  - papers
tags:
  - DL
  - Generalization
---
# [Flooding]  Do We Need Zero Training Loss After Achieving Zero Training Error?

Author: Takashi Ishida
Publish: ICML
Status: Finished
Sub-Topic: Overfitting
Topic: DL
Year: 2020

# [Flooding]  Do We Need Zero Training Loss After Achieving Zero Training Error?

- ICML 2020,  [https://arxiv.org/abs/2002.08709](https://arxiv.org/abs/2002.08709) 
- 느낀 점 : overfitting을 막는 새로운 방법이기 때문에 accept된 것처럼 보이나 이에 대한 이론적인 배경이나 서술이 부족하다고 생각됨.  방법이 매우 간단해서 사용할 수 있는 방법은 많을 것으로 보임.

## Abstract

1. the training loss continues to approach zero, making the model overconfident and the test performance degraded → zero training loss가 문제다!
2. maintain a fixed/preset level of training loss (flood level)

## Introduction

![Untitled.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled.png){:.center-image}

1. float around a small constant value
2. Algorithm

![Untitled%201.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%201.png){:.center-image}

    - $b>0$ : flood level specified by the user
    - learning objective is above the flood level
    - "gravity" effeect with gradient descent when objective is above the flood level

        "buoyancy" effect with gradient ascent when objective is below the flood level

3. b: hyperparameter (decided by experiments)

![Untitled%202.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%202.png){:.center-image}

## Backgrounds

1. Other methods to prevent overfitting

![Untitled%203.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%203.png){:.center-image}

2. double descent curves with Overparametrization (Belkin et al. (2019)

- test error as a function of model complexity decreases with low model complexity but starts to increase after the model complexity is large enough.
- excessive complexity leads to poor generalization

![Untitled%204.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%204.png){:.center-image}

→ was oberserved in experiemnts after using flooding with only about 100 epochs (더 자세한 건 out of scope라고 서술)

## Experiment setting

1. loss as softmax cross-entropy loss
2. varirous of $b \in (0,0.01,0.02, ..., 0.20)$ : hyperparameter
3. mini-batch stochastic optimization

![Untitled%205.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%205.png){:.center-image}

## Result

1. 일단 flooding의 비교 대상을 early stopping으로 잡음.
2. early stopping이 없을 때는 overfitting이 일어나므로, flooding을 사용하는 것이 성능을 향상시킴. Early stopping을 사용하더라도, flooding을 사용하면 더 좋은 결과를 얻을 수 있음.

![Untitled%206.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%206.png){:.center-image}

3. 그렇다면, early stopping과 flooding 중에 뭐가 더 좋은 방법일까? 라는 궁금증이 생기기 때문에 W(weight decay), E(early stopping), F(flooding)의 조합으로 여러 데이터셋에 대해 실험.

    → 각 데이터 셋마다 어떤 방법이 효율적인지가 다르다. 예를 들어 MNIST는 early stopping은 큰 효과가 없으나 weight decay는 큰 효과가 있으며, flooding도 잘 적용됨. 반면, CIFAR-10은 셋 다 쓰는 것이 훨씬 효율적.

![Untitled%207.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%207.png){:.center-image}

![Untitled%208.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%208.png){:.center-image}

4.  성능이 early stopping에 비해 엄청 높지는 않지만 model depency라던지 제약이 적기 때문에 general 하게 사용할 수 있다.
5. train loss and test loss w, w/o flooding

![Untitled%209.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%209.png){:.center-image}

## Why Does Flooding Generalize Better?

1. memory
    - model will give up memorizing all training datas as the flood level becomes higher
    - **marked positiion (chosen based on validation accuracy) often plotted at zero training error (training error는 낮은 게 generalization에도 좋다)**

![Untitled%2010.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%2010.png){:.center-image}

    2. gradient amplitude ($\ell_2$norm of the filter-normalized gradient of the loss)

    - gradient amplitude is the $\ell_2$ norm of the filter-normalized gradient of the loss.
    - zero training error leads to lower generalization error → **zero training loss may be harmful under zero training error (loss가 0인 것은 오히려 좋지 않다).**
    - flooding helps to escape local minima

     → 아래 그림을 보면 flooding 한 것은 epoch이 증가함에 따라 test loss가 줄지만, overfitting은 반대

![Untitled%2011.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%2011.png){:.center-image}

3. flat loss (sharp minima보다 flat minima가 좋다)

![Untitled%2012.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%2012.png){:.center-image}

4. MSE is smaller in flooding (flooding의 loss가 더 작다)

![Untitled%2013.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%2013.png){:.center-image}

![Untitled%2014.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%2014.png){:.center-image}

![Untitled%2015.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%2015.png){:.center-image}

## Overfitting in adversarially robust deep learning

- 이 논문은 Rice가 쓴 overfitting을 막는 방법으로 early stopping을 사용한 논문

![Untitled%2016.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%2016.png){:.center-image}

1. 논문은 pgd 10 steps

![Untitled%2017.png]({{site.url}}/assets/images/2021-7-14-Do We Need Zero Training Loss After Achieving Zero Training Error/Untitled%2017.png){:.center-image}

2. cifar 10 같은 기준으로 돌렸을 때 acc 0.5759 (error 42.41) b=1.15 나옴 (197/200 steps)

- adversarial attack 분야에서도 overfitting을 막는 방법으로 사용될 수 있음
(early stopping + flooding 가능)
- 그러나 성능의 큰 향상은 아닌 것 같기도 하고, 이론적으로 설명하기가 너무 애매
- b를 설정하는 게 standard training보다 훨씬 크게 설정해야 하며, 이에 따라 차이가 좀 많이 난다는 점이 hyperparameter setting의 어려움도 있음.
