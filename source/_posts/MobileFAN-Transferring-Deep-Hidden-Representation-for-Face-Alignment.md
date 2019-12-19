---
title: 'MobileFAN: Transferring Deep Hidden Representation for Face Alignment'
date: 2019-12-16 22:18:39
tags:
  - Landmark
  - Face
  - Mimick
---
URL: https://arxiv.org/abs/1908.03839

一篇人脸关键点的论文,论文的核心应该是模型蒸馏，但是单纯的基于mobilenetv2网络在多个benchmark也刷了挺不错的点，甚至是好于CVPR2018的LAB，MobileFAN就是mobilenetv2加上出heatmap直接出出来的结果，MobileFAN + KD就是加上只是蒸馏的结果，下表是WFLW上面的点,坦白讲点还是很高的：
![](MobileFAN-Transferring-Deep-Hidden-Representation-for-Face-Alignment-屏幕快照 2019-08-16 下午6.41.34.png)
知识蒸馏方面论文中提了两个方面：
一是**Feature-Aligned Distillation**，出发点是希望teacher和student网络学习的分布是一致的，两个网络deconv层的spatial维度是一致的差别就在于channel不一致，作者简单的用1x1把两者拉统一，Feature-Aligned Distillation和之前用大模型带小模型的做法基本是一致的，学习的时候直接mse监督两个feature map。
![](MobileFAN-Transferring-Deep-Hidden-Representation-for-Face-Alignment-Screenshot from 2019-08-15 10-54-20.png)
另一个是**Feature-Similarity Distillation**，这部分主要想让teacher和student网络表示的空间信息是一致的，比如人脸结构轮廓等，这一部分首先teacher和student网络分别去计算不同pixel之间的相似度：
![](MobileFAN-Transferring-Deep-Hidden-Representation-for-Face-Alignment-截屏2019-12-1622.24.41.png)
然后两个网络feature map之间计算相似度：
![](MobileFAN-Transferring-Deep-Hidden-Representation-for-Face-Alignment-截屏2019-12-1622.24.46.png)
总之感觉从结果上看还是挺惊艳的，模型蒸馏感觉对于小模型可以好好搞一下。
