---
title: Adversarial Training with Stochastic Weight Average 논문 요약
excerpt: Joong-Won Hwang외의 'Adversarial Training with Stochastic Weight Average' 논문을 요약한 글입니다.
categories:
  - papers
tags:
  - Adversarial attack
---
# [SWAAT] Adversarial Training with Stochastic Weight Average

Author: [ETRI] Joong-Won Hwang, Youngwan Lee, Sungchan Oh, Yuseok Bae
Keywords: SWA
Publish: AAAI
Status: Finished
Sub-Topic: Adversarial Training
Topic: Adv
Year: 2021

## Introduction

### Ensemble methods and overfitting

- adversarial training is prone to overﬁtting
- one way to relieve overﬁtting from the lack of data is to use ensemble methods
- dilemma on choosing models to generate adversarial examples
: individual members or whole ensemble system

    dilemma of decoupling

![Untitled.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled.png){:.center-image}

### Related work

- Revisiting FGE (fast geometric ensembling) : saves check- points from different moments of training and forms the en- semble using it at the inference stage. In other word, the tem- poral checkpoints during training becomes the members of ﬁnal ensemble → inefficient

    computational cost to be remain similar to single model though it has performance of ensemble

- Grefenstette et al. 2018; Wang, Shi, and Osher 2019
:they train the members of ensemble jointly with the adversarial examples generated with respect to the whole ensemble model. In other word, they train the ensemble model like single huge network.

## Method

- $x_{adv}(m(\theta),x)=argmax_{x' \in B(x)}L(m_i(\theta_i,x'),y)$

    $\theta=\theta_j-\eta\nabla_\theta L(m_j(\theta_j,x_{adv}),y)$

- whole ensemble $E(x)={1 \over l}\sum^{l-1}_{i=0}m_i(\theta_i,x)$

![Untitled%201.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled%201.png){:.center-image}

### Dillema of Decoupling

: use $x_{adv}(E,x)$  instead of $x_{adv}(m_i,x)$ : white box에 취약하지 않도록

- pros: ensemble의 장점 획득 가능
- cons: degenerates the diversity of members, each member $m_i$ have week robustness

### Stochastic weight averaging

- utilize single network with averaged weight instead of averaging multiple networks
- model is updated with the aggregated weight at the end of the training
- one important difference in SWAAT from SWA is that weight of the model is updated to aggregated weight $\theta_{swa}$ at the end of every epoch instead of final epoch.

![Untitled%202.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled%202.png){:.center-image}

![Untitled%203.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled%203.png){:.center-image}

![Untitled%204.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled%204.png){:.center-image}

### Batch Normalization Statistic

- When weight states are averaged in SWA, mean and standard deviation of hidden response in network should be recalculated respectively.
- adversarial examples are generated in evaluation mode which uses the statistic of the whole data.

### Diversity via Data Resampling

- AT overfitting: slight change in training dataset can introduce diversity on members
: resample the data from the original dataset with replacement
1. resampling with bootstrapped (BOOT) : a random sampling
2. resampling with hard example mining (HEM) : evaluate 할 때 train set도 검사해서 $E$로 misclassification한 sample mark → next epoch에서 triple probability to be sampled (compuatation cost 증가)
3. online-hard example mining (OHEM) : HEM의 misclassification을 train할 때 검사 (cost 동일)

## Experiments

### Settings

- SWAAT for 100 epochs on CIFAR-10 and CIFAR-100 to ﬁx the total training length to 200 epochs
- SWAAT-window is set to four
- 10 step PGD with $\alpha=2/255,\epsilon=8/255$
- $\ell_\infty$ constraint

### Experiment result

- 아래 table이 비교한 성능

    PGD-50이나 AA가 없어서 비교할 수는 없으나...

![Untitled%205.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled%205.png){:.center-image}

- Revisiting Bags of Tricks (AA 54.39는 semi supervised 안쓴것 중에 굉장히 높은 성능)

![Untitled%206.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled%206.png){:.center-image}

- Resampling 방법론을 하면 성능이 증가

![Untitled%207.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled%207.png){:.center-image}

![Untitled%208.png]({{site.url}}/assets/images/2021-7-14-Adversarial Training with Stochastic Weight Average/Untitled%208.png){:.center-image}

## Conclusion

- most effective ensemble method with efficient memory and time
- 성능이 좋은건지 확인해볼 필요 있음
