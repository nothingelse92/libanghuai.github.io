---
title: >-
  IGCV3: Interleaved Low-Rank Group Convolutions for Efficient Deep Neural
  Networks
date: 2019-01-20 21:23:54
tags:
  - Basemodel
---
URL: https://arxiv.org/abs/1806.00178
IGCV3，BMVC2018，在IGCV2基础上进行的改进，主要引入bottleneck的网络结构来改善IGCV2网络的信息交互，从而进一步提高网络效果。
在IGCV2中曾提到过一个互补条件：前面一级group convolution里不同partition的channel在后面一级的group convolution要在同一个partition里面，那么follow这个逻辑设计的IGCV2网络结构的每一个输出channel、输入channel之间将有且仅有一条路径可以连接，示意图如下，这就会导致网络过于稀疏不一定有利于网络的整体性能：
![](IGCV3-Interleaved-Low-Rank-Group-Convolutions-for-Efficient-Deep-Neural-Networks-a0f037095b27476caa3cf49ca30929b2923f2315_1_690x253.jpeg)

那么在IGCV3网络设计中借助bottleneck结构提出了super-channel的概念，每一个super-channel其实就是一组channel，first group convolution是1x1的卷积，它把网络变宽，second group convolution是3x3的spatial conv，third group convolution 同样是1x1的卷积，它又把网络结构压缩到原来的宽度，整个过程中的permutation和IGCV1、IGCV2的逻辑是一致的，这样设计之后每一个输出和输入的channel之间都会有多条路径可以交互，这样就优化了IGCV2网络设计中的问题，具体逻辑如下图：
![](IGCV3-Interleaved-Low-Rank-Group-Convolutions-for-Efficient-Deep-Neural-Networks-dfcbf9fea9f926a00f49a63e9242ce7a83ec10e8_1_690x316.png)
IGCV1、IGCV2、IGCV3在CIFAR上面的对比：
![](IGCV3-Interleaved-Low-Rank-Group-Convolutions-for-Efficient-Deep-Neural-Networks-2460c1c050b642ec41193bbc0f29770f65933ac2_1_690x242.png)
和MobileNetV2的对比：
![](IGCV3-Interleaved-Low-Rank-Group-Convolutions-for-Efficient-Deep-Neural-Networks-屏幕快照 2019-01-19 上午11.38.42.png)
和其他现有针对移动平台设计的网络对比：
![](IGCV3-Interleaved-Low-Rank-Group-Convolutions-for-Efficient-Deep-Neural-Networks-屏幕快照 2019-01-19 上午11.39.27.png)
