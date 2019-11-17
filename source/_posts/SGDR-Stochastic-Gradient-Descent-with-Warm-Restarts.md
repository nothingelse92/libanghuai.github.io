---
title: 'SGDR: Stochastic Gradient Descent with Warm Restarts'
date: 2018-11-24 22:47:32
tags:
  - Learning Strategy
---
URL: https://arxiv.org/abs/1608.03983 Github: https://github.com/loshchil/SGDR
**【Summary】**ICLR2017一篇关于学习率的论文，论文的核心比较直接就是提出了基于cosine的学习率 warm restart逻辑，然后论文的大篇幅都是围绕这个learing rate进行了比较多的实验。论文所提的SGDR通常只需要原有模型1/2-1/4的训练epoch就可以得到差不多甚至更好的效果。

论文所提的cosine learning rate 公式如下，n<sub>min</sub>、n<sub>max</sub>就是学习率的区间，T<sub>cur</sub>表示当前经过多少个epoch了，T<sub>i</sub>可以理解为周期。因为从下面这个公式当T<sub>cur</sub> = T<sub>i</sub>时，n<sub>t</sub> = n<sub>min</sub>：

![](SGDR-Stochastic-Gradient-Descent-with-Warm-Restarts-c4d6e75499fcd3406791dd38969c8d52b2f59521_1_690x119.png)
作者在具体实验的时候其实还涉及到另一个参数n<sub>mult</sub>代表周期的系数，下图是不同的T<sub>i</sub>和T<sub>mult</sub>画出来的示意图：

![](SGDR-Stochastic-Gradient-Descent-with-Warm-Restarts-d82c1f3a92421e0ca83d33a2f7b0fe29374e7d62_1_690x288.jpg)

下图是作者在CIFAR-10和CIFAR-100数据集上用Wide Residual Neural Network（depth=28&width=10）做的实验，效果很直观，SGDR收敛速度明显比较快：

![](SGDR-Stochastic-Gradient-Descent-with-Warm-Restarts-76589c0bc925d1e5325639557cd6ecdb400389da_1_447x499.jpg)
下表是具体的实验结果，从结果上看效果也是不错的：

![](SGDR-Stochastic-Gradient-Descent-with-Warm-Restarts-4441390556648037aa6d983e2edfb07a2a075d6a_1_629x500.png)
此外作者也从ensemble、downsampled imagenet dataset的角度做了不同的实验，结果也都很好：
Ensemble:

![](SGDR-Stochastic-Gradient-Descent-with-Warm-Restarts-f2310a3f482911fcebf25cb90947d106fcf3e22a_1_690x130.png)
Downsampled imagenet dataset:

![](SGDR-Stochastic-Gradient-Descent-with-Warm-Restarts-d308a8fb8fb5028f53e7d108006f93a5d5364a52_1_632x500.jpg)

论文所提的这种warm restart逻辑感觉还是很合理的，可以优化梯度下降中鞍点以及局部最小解的问题，有助于模型能快速收敛。
