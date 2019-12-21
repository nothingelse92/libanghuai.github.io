---
title: 'AAR-CNNs: Auto Adaptive Regularized Convolutional Neural Networks'
date: 2019-07-16 22:37:50
tags:
  - Regularization
  - Attention
---

IJCAI2018一篇关于regularization的论文，论文的motivation是想在更细的维度做regularization，BN可以理解为batch维度的regularization，而本文提出的AAR-CNN则可以理解为channel维度和pixel维度的regularization，论文结合了SE-Net的相关内容，整篇看来其实可以理解为pixel（channel）维度的attention。

1. AE-Net：Abstract Extent（AE）直译过来就是抽象程度的意思，作者认为CNN网络中不同的layer的抽象程度是不一样的，论文中也分别从pixel level和channel level来定义了AE。对于pixel level的AE，假设输入为(s1,s2,s3,s4), s1为mini-batch size，s2为channel，s3为h，s4为w，初始化AE的表达为(1,1,s3,s4),然后通过conv (1,s1,1,1) + dimshuffle + conv(1,s2,3,3)得到最后的输出v2(s1,s2,s3,s4),最后做pixel wise的tanh就得到最后的AE的表达了，channel level的做法类似通过两层FC得到：

 ![](AAR-CNNs-Auto-Adaptive-Regularized-Convolutional-Neural-Networks-thumbnail_image002.png)

2. SE-Net：这一部分就是完全来自于之前读的Squeeze-and-Excitation Networks这篇论文，对于输入的每一个channel加attention：

  ![](AAR-CNNs-Auto-Adaptive-Regularized-Convolutional-Neural-Networks-thumbnail_image003.png)

所以整个pipeline就可以整合成如下的示意图，AE-net和SE-net的输出相乘就是最后regularization的表达，将其施加于输出之上产生的结果再和输出sum得到最后的输出：

![](AAR-CNNs-Auto-Adaptive-Regularized-Convolutional-Neural-Networks-thumbnail_image004.png)
