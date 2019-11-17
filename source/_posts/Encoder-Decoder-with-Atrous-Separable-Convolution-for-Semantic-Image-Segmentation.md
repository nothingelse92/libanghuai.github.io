---
title: >-
  Encoder-Decoder with Atrous Separable Convolution for Semantic Image
  Segmentation
date: 2019-01-28 13:26:57
tags:
  - Segmentation
---
URL: https://arxiv.org/pdf/1802.02611.pdf
这是DeepLab系列的第四篇文章，DeepLab V3+，这篇论文的主要内容主要是在DeepLab V3的基础上接了一个decoder形成一个hourglass的结构结果直接出原图的resolution，这相比前面版本直接插值要更好一点，整个结构中encoder就直接用的DeepLab V3的结构，输出结果concat到一起之后会和低层的网络特征直接concat，低层的特征通过1x1卷积来降维，简单的示意图：
![](Encoder-Decoder-with-Atrous-Separable-Convolution-for-Semantic-Image-Segmentation-屏幕快照 2019-01-26 上午11.18.27.png)

论文中另外一个主要内容是depthwise separable convolution的使用，论文中用深度可分离的空洞卷积来替换掉原有xception中的max pooling 层，主要也是出于模型速度的优化，整体的网络结构：
![](Encoder-Decoder-with-Atrous-Separable-Convolution-for-Semantic-Image-Segmentation-屏幕快照 2019-01-26 上午11.29.11.png)
