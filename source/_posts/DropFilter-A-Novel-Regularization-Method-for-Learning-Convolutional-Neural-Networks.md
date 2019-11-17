---
title: >-
  DropFilter: A Novel Regularization Method for Learning Convolutional Neural
  Networks
date: 2018-12-19 23:53:33
tags:
  - Detection
  - Regularization
---
URL: https://arxiv.org/abs/1811.06783
论文主要提出了一种新的Regularization方法DropFilter，主要是对卷积核进行抑制，具体实现的时候也比较简单，只要对卷积核和bias生成对应的0 - 1mask(Bernoulli分布),然后对应和kernel相乘即可：

![](DropFilter-A-Novel-Regularization-Method-for-Learning-Convolutional-Neural-Networks-6cc74f13d759b1cbb0c4a864d637b520623e0053_1_690x465.png)
在DropFilter的基础上作者还提出了DropFilter-PLUS方法，主要是针对卷积操作过程中的每一个位置都进行一次独立的DropFilter操作，那么对于一个224x224的输入需要计算222x222次mask操作(3x3conv)，计算量比较大，作者在论文中分析具体的conv操作(论文中提及的alx,y)最后通过对卷积的输出进行mask来等价，这就大大加速了DropFilter-PLUS的操作：
![](DropFilter-A-Novel-Regularization-Method-for-Learning-Convolutional-Neural-Networks-9fa3f026fff5f15f159490c18a254286219fda82_1_585x500.png)
DropFilter和Dropfilter-PLUS都有一定的道理，并且简单易实现
