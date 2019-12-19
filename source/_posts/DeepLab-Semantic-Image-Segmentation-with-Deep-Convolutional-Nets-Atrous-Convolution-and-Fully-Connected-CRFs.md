---
title: >-
  DeepLab: Semantic Image Segmentation with Deep Convolutional Nets, Atrous
  Convolution, and Fully Connected CRFs
date: 2019-01-28 13:17:35
tags:
  - Segmentation
  - DeepLab
---
URL: https://arxiv.org/abs/1606.00915
DeepLab系列的第二篇论文，主要是在DeepLab V1的基础上提出了ASPP的结构来做多尺度的问题。
在DeepLab V1中作者其实也提到了multiscale的问题，当时论文中提到的方法就是对输入进行多次rescale，那么这种方法很显然太naive，并且也会带来比较大的运算量。因此DeepLab V2中作者就借鉴SPP的方法提出了ASPP的结构来解决多尺度的问题，ASPP本质就是用不同rate的空洞卷积来实现不同的感受野这样就可以覆盖比较多scale的物体，具体的应用逻辑论文中也给出了具体的结构：
![](DeepLab-Semantic-Image-Segmentation-with-Deep-Convolutional-Nets-Atrous-Convolution-and-Fully-Connected-CRFs-屏幕快照 2019-01-25 下午11.01.55.png)

![](DeepLab-Semantic-Image-Segmentation-with-Deep-Convolutional-Nets-Atrous-Convolution-and-Fully-Connected-CRFs-20181220234952.png)
