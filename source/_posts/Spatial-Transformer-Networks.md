---
title: Spatial Transformer Networks
date: 2019-04-10 13:21:15
tags:
  - Detection
  - Classic
---
URL:https://arxiv.org/pdf/1506.02025.pdf
这是一篇NIPS 2015的文章，主要提出了STN网络结构直接的赋予了网络对于各种变换的不变性。
STN网络主要分为三个部分：
1. Localisation Network：一个子网络-用来学习变换参数θ ，θ的大小则和具体的变换有关，比如一般的仿射变换就是6维的参数，子网络的形式可以是FC也可以是CNN结构。
2. Parameterised Sampling Grid：这一部分负责将目标feature map和 源 feature map的像素之间形成映射，主要计算目标feature map的每一个位置在源feature map上对应的位置 TG。
![](Spatial-Transformer-Networks-image001.png)
3. Differentiable Image Sampling：这一部分主要根据Parameterised Sampling Grid生成的TG和源feature map信息采样得到目标feature map.
![](Spatial-Transformer-Networks-image002.png)
一些实验结果：
![](Spatial-Transformer-Networks-image003.png)
![](Spatial-Transformer-Networks-image004.png)
