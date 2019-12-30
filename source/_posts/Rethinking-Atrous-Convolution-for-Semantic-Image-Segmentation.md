---
title: Rethinking Atrous Convolution for Semantic Image Segmentation
date: 2019-01-28 13:21:19
tags:
  - Segmentation
  - DeepLab
---
URL: https://arxiv.org/pdf/1706.05587.pdf
这篇论文是DeepLab系列的第三篇论文DeepLab V3. 相比较DeepLab V1和V2主要在研究如何更好的运用空洞卷积, 同时也去掉了之前的DenseCRF后处理模块。论文中主要涉及了两个方面一个是串行的连接空洞卷积，因为相比较传统的直接用conv层来增加网络的深度会导致抽象到最后一层会丢掉很多的原始信息，利用空洞卷积可以一定程度上优化这个问题。另一方面是在ASPP的基础上优化空洞卷积的使用，作者通过实验发现，如果一味的通过扩大空洞卷积的rate来扩大感受野，有时候并不能得到很好的结果，比如下图就说明当rate足够大的时候其实卷积核有效的参数量只有一个：
![](Rethinking-Atrous-Convolution-for-Semantic-Image-Segmentation-屏幕快照 2019-01-26 上午11.08.28.png)

所以最后作者在ASPP模块中就直接用全图的global average pooling来替代空洞卷积来获取比较大区域的特征：
![](Rethinking-Atrous-Convolution-for-Semantic-Image-Segmentation-20181221233024.png)
