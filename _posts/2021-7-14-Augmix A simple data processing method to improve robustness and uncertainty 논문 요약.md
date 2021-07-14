---
title: Augmix A simple data processing method to improve robustness and uncertainty 논문 요약
excerpt: ICLR 2020에 발표된 Dan Hendrycks외의 'Augmix: A simple data processing method to improve robustness and uncertainty' 논문을 요약한 글입니다.
categories:
  - papers
tags:
  - DL
---
# AUGMIX: A SIMPLE DATA PROCESSING METHOD TO IMPROVE ROBUSTNESS AND UNCERTAINTY

Author: (Deepmind) Dan Hendrycks, Norman Mu, Ekin D. Cubuk, Barret Zoph, Justin Gilmer, Balaji Lakshminarayanan
Publish: ICLR
Status: Finished
Sub-Topic: DL
Topic: DL
Year: 2020

- [https://robustbench.github.io/#leaderboard](https://robustbench.github.io/#leaderboard) 에서 Common Corruptions Cifar 10, Cifar 100에서 높은 등수를 유지하고 있는 논문

# Introduction

### training distribution $\neq$ test distribution

- machine learning methods가 new scenarios를 봤을 때 trainig한 distribution과 같을 가능성 낮다.
- mismatches가 흔하지만, 이에 대한 연구는 진행 X → model 이 test distribution에 따라 robust X
- overconfident prediction → miscalibration

### speicific corruption에 대한 학습

- training against corruption → encourage networks to memorize the specific corruption
- unable to generalize to new corruption
- (ex) translation에 대해 학습한 모델은 single fixel만 옮겨도 attack 가능
- ImageNet-C, CIFAR10-C, CIFAR100-C

![Untitled.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled.png){:.center-image}

![Untitled%201.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%201.png){:.center-image}

# Related Work

### AutoAugment(Cubuk et al ., 2018 CVPR) Google

- 방법 자체는 RNN+강화학습을 사용해 additional data를 안쓰고 standard training의 성능을 높이는 논문 (data augmentation)

![Untitled%202.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%202.png){:.center-image}

![Untitled%203.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%203.png){:.center-image}

AutoAugment로 generate

→ 위의 ImageNet-C와 다르다

- 각 데이터셋에서 유의미한 data augmentation을 강화학습으로 찾고, 이를 통해 학습 데이터셋을 부풀려서 학습 (ex, On CIFAR-10, AutoAugment picks mostly color-based transformations.)
- 번외로 찾아보니 카카오브레인에서 Fast  AutoAugment을 제안해 속도를 높여 2019 NeurIPS 2019
- **CIFAR10-C에 있는 augmentation operation으로 학습하면 그 augmentations 에 dependent 하게 학습되므로 아예 다른 방법으로 augmentation 생성하겠다!**

# AUGMIX

### Algorithm

![Untitled%204.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%204.png){:.center-image}

![Untitled%205.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%205.png){:.center-image}

![Untitled%206.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%206.png){:.center-image}

### Augmentation operation

- 위에서 설명한 AutoAugment 사용
- CIFAR10-C와 겹치는 contrast, color, brightness, sharpness, Cutout은 사용하지 않음

    → operations and ImageNet-C corruptions are disjoint

- k augmentation chains (k=3 default), 각 chain은 one to three randomly selected augmentation

### Mixing

- mixing of weights (w → Dirichlet distribution, m → beta distribution)

### Jensen-Shannon Divergence Consistency Loss

- Jensen-Shannon divergence among the posterior distributions
- $p_{orig}=\hat{p}(y|x_{origin})$, $p_{augmix1}=\hat{p}(y|x_{augmix1})$, $p_{augmix2}=\hat{p}(y|x_{augmix2})$

![Untitled%207.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%207.png){:.center-image}

![Untitled%208.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%208.png){:.center-image}

![Untitled%209.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%209.png){:.center-image}

$JS(p_{orig};p_{augmix1})$ 만도 실험할 수 있는데 이는 결과가 좋지 않았다 (why?)

# EXPERIMENTS

### Datasets

- CIFAR10-C, ImageNet-C with blur, weather, digital corruption (15 noise with 5 levels)

### Metric

- **Error**
    - Corrupted data는 sensitivity에 따라 그 error rate가 다르기 때문에 $E_{c,s}$
    - $CE_{c}=\sum_{s=1}^5E_{c,s}/\sum_{s=1}^5E_{c,s}^{Alexnet}$ 즉, base model로 잡은 Alexnet의 E에 비해 얼마나 차이나는지
    - $mCE$(mean corruption error) = 15 corruption의 평균
- Flip probability → for Perturbation robustness **(lower is better)**
    - ex) with videos such as those with steadily increasing brightness , measure two adjacent frames, or two frames with slightly different brightness levels, have “flipped” or mismatched predictions. (인접한 frame이 flip되었는지를 잰다)
- calibration → capable of reliable forecasting their accuracy (Brier Score와 연관)
    - a calibrated classifier should be correct 70% of the time on examples to which it assigns 70% confidence
    - RMS Calibration Error : squared difference between the accuracy at a given
    confidence level and actual the confidence level. **(lower is better)**

![Untitled%2010.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%2010.png){:.center-image}

아래 두 metric은 잘하는 걸 보여주기 위함. 

# Results

### CIFAR-10-C

![Untitled%2011.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%2011.png){:.center-image}

다른 Data augmentation 방법론들은 정확한 방법은 없지만 아마 Generated된 Data까지 사용해서 학습한 결과가 아닐까?

- Flip probability and Calibration on Clean and Corrupted Data

![Untitled%2012.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%2012.png){:.center-image}

### Imagenet

- Stylized ImageNet (SIN)
    - technique where models are trained with the original ImageNet images and also ImageNet images with style transfer applied

![Untitled%2013.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%2013.png){:.center-image}

### 추가 실험

- Ensemble하면 더 잘한다
- Fourier Sensitivy analysis
    - add a total of 32X32 Fourier basis vectors to the CIFAR-10 test set
    - Each point in the heatmap shows the error rate on the CIFAR-10 test set after it has been perturbed by a single Fourier basis vector

![Untitled%2014.png]({{site.url}}/assets/images/2021-7-14-Augmix A simple data processing method to improve robustness and uncertainty 논문 요약/Untitled%2014.png){:.center-image}
