---
title: Deep Layer Aggregation
date: 2018-12-17 20:40:01
tags:
  - Detection
---
URL: https://arxiv.org/pdf/1707.06484.pdf
CVPR2018的一篇关于layer aggregation的论文，论文的motivation是作者觉得目前常见的aggregation方式（FPN、U-Net...）比较shallow，作者希望利用更加deeper的连接方式来更好的融合特征。论文中作者分别提出了IDA和HDA两种连接方式。IDA应用在stage之间，HDA应用在block之间。

下图是论文中对IDA和HDA给出的直观图示，下图的c是IDA（Iterative Deep Aggregation），整体和FPN的连接方式比较类似，只是方向相反，浅层的特征被不断的refine与高层特征相融合。下图的d，e，f对应着HDA（Hierarchical Deep Aggregation），d是HDA最原始的表达，整体是一个树状结构，跨越不同层级的特征分层次进行特征融合，e则是在d的基础上将前面节点的父亲节点与当前节点一同考虑进行特征融合。f则是作者出于降低模型复杂度的角度将e同一层级的节点进行merge：
![](Deep-Layer-Aggregation-屏幕快照 2018-12-17 下午8.41.22.png)
下面这幅图是论文中将IDA和HDA合并到一起具体的模型应用，也就是论文标题的Deep Layer Aggregation模型，橙色的连线是IDA的逻辑，红色框内部的连接是HDA的逻辑：
![](Deep-Layer-Aggregation-屏幕快照 2018-12-17 下午8.41.53.png)
而对于一些比如seg的‘image-to-image’任务，只需通过增加简单的插值操作就可以实现：
![](Deep-Layer-Aggregation-屏幕快照 2018-12-17 下午8.42.30.png)
为了验证DLA的有效性作者还是做了比较多的实验的，作者分别在Classification、Fine-grained Recognition、Seg、Boundary Detection几个主流的cv task上都做了实验，都有不同程度的提升：
分类的实验：
![](Deep-Layer-Aggregation-屏幕快照 2018-12-17 下午8.48.56.png)
识别的实验：
![](Deep-Layer-Aggregation-屏幕快照 2018-12-17 下午8.49.01.png)
论文中作者提出的IDA和HDA感觉还是很合理的，作者同时还提及residual connection在实验时对比较深的网络是有效的，对于比较浅的网络是有负面作用的。
