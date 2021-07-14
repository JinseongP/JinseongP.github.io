---
title: Averaging Weights Leads to Wider Optima and Better Generalization
excerpt: UAI 2018에 발표된 Pavel Izmailov외의 'Averaging Weights Leads to Wider Optima and Better Generalization' 논문을 요약한 글입니다.
categories:
  - papers
tags:
  - DL
  - generalization
---
# [SWA] AveragingWeights Leads to Wider Optima and Better Generalization

Author: Pavel Izmailov, Dmitrii Podoprikhin, Timur Garipov, Dmitry Vetrov, Andrew Gordon Wilson
Publish: UAI (Conference on Uncertainty in Artificial Intelligence)
Status: Finished
Sub-Topic: Overfitting
Topic: DL
Year: 2018

# Introduction

### Related Work:
Fast Geometric Ensembling (FGE) : ensemble models' predictions

- Loss Surfaces, **Mode Connectivity**, and **Fast Ensembling** of DNNs (Garipov et al., 2018)
- Mode connectivity: local optima found by SGD can be connected by simple curves of near constant loss
- sample multiple nearby points in weight space to create high performing ensembles
- high frequency cyclical learning rate with SGD to select networks to ensemble
- Cyclic Cosine Annealing을 통해 일정 주기마다 Learning Rate를 다시 크게 키워주면서, minimum learning rate의 모델을 저장해서 training이 끝난 이후 ensemble

![Untitled.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled.png){:.center-image}

- Similar to “Snapshot ensembles: Train 1, get m for free”, 2017 ICLR

![Untitled%201.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%201.png){:.center-image}

FGE needs more memories since it needs multiple models

### Proposed Method:
Stochastic Weight Averaging (SWA) : ensemble models' weights

- Loss Surfaces, **Mode Connectivity**, and **Fast Ensembling** of DNNs (Garipov et al., 2018)
- Since FGE needs more computation for ensemble k models, SWA is memory and time efficient.

Consequently, SWA can be interpreted as an approximation to FGE

- SWA solution is not a local optimum of the loss

Train loss and test error surfaces are qualitatively similar, they are not perfectly aligned

![Untitled%202.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%202.png){:.center-image}

- SGD generally converges to a point near the boundary of the wide ﬂat region of optimal points

Since ensembling SGD, SWA has flatter minima

→ **Flat minima를 찾는 방법!**

# Method

### Stochastic weight averaging

- update weight of a single model

![Untitled%203.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%203.png){:.center-image}

![Untitled%204.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%204.png){:.center-image}

### Learning rate cycle

- capture the models that correspond to the minimum values of the learning rate

![Untitled%205.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%205.png){:.center-image}

# Experiments

### Gap between Test error and Train loss

- L2 -regularized cross-entropy train loss and test error as a function of a point on the line connecting SWA and  SGD  solutions  on CIFAR-100.  
Left:  Preactivation  ResNet-164.   Right:  VGG-16.

![Untitled%206.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%206.png){:.center-image}

In left figure, we can see the test error of SGD is higher even if train loss is lower.

SWA has flat minima

### Experiment result

![Untitled%207.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%207.png){:.center-image}

- Table result (Budget : the number of epochs required to train the model until convergence with conventional SGD training)

![Untitled%208.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%208.png){:.center-image}

# Conclusion

- PyTorch 1.6 버전에서 공식적으로 지원

![Untitled%209.png]({{site.url}}/assets/images/2021-7-14-Averaging Weights Leads to Wider Optima and Better Generalization/Untitled%209.png){:.center-image}

    ```python
    from torchcontrib.optim import SWA

    ......# training loopbase_opt = torch.optim.SGD(model.parameters(), lr=0.1)
    opt = torchcontrib.optim.SWA(base_opt, swa_start=10, swa_freq=5, swa_lr=0.05)
    for _ in range(100):
         opt.zero_grad()
         loss_fn(model(input), target).backward()
         opt.step()
    opt.swap_swa_sgd()
    ```

- i**mproves generalization performance at virtually no additional cost over conventional training.**
