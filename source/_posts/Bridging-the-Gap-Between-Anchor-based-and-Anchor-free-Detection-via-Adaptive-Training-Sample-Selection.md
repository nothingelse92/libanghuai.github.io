---
title: >-
  Bridging the Gap Between Anchor-based and Anchor-free Detection via Adaptive
  Training Sample Selection
date: 2020-02-18 10:55:11
tags:
  - Object Detection
  - Label Assign
---
URL: https://arxiv.org/pdf/1912.02424.pdf

Label Assign另一篇比较有代表性的工作, 论文主要想探究Anchor-based方法和Anchor-free方法本质的差异性(这里主要考虑one stage的RetinaNet和FCOS)，结论是**正负样本取样的差异性导致的**。

作者首先对比了一下RetinaNet和FOCS点上面的差异，加上一堆trick之后，RetinaNet AP能到37.0%，而FCOS能到37.8%，之间有0.8%的GAP，那么就其本身这两个方法现在就剩两个不一样的地方了：

1. 正负样本定义不一样，RetinaNet是anchor-based，通过卡IOU来区分正负样本，每个pixel有多个不同的anchor，FCOS是基于点的，每个点有一个anchor point(正or负)，取决于gt框的大小和不同layer定义的回归scale。
2. 回归的方式不一样，FCOS从**anchor point**回归，RetinaNet从**anchor box**回归。

针对上面提到的不同的两点作者也做了一个实验，RetinaNet采用FCOS的策略可以把点从37.0%涨到37.8%，而如果FCOS采用RetinaNet的策略点就会从37.8%下降到36.9%：

![](Bridging-the-Gap-Between-Anchor-based-and-Anchor-free-Detection-via-Adaptive-Training-Sample-Selection-屏幕快照 2020-02-18 下午4.10.40.png)

那么横向看上面这张表，可以方面无论是基于anchor point回归还是基于anchor box回归点是差不多的，所以作者得出结论，RetinaNet和FCOS点有差的最大原因就是**正负样本取样的差异性导致的**.

然后就是论文的核心ATSS(Adaptive Training Sample Selection)策略了,解决*how to define positive and negative training samples*的问题.
具体怎么做呢：
1. 对于给定的gt框，在FPN的每一个layer上找Top K个离gt框中心最近的anchor box(距离的话就用两个框中心点的L2距离衡量)作为正样本，那么假设FPN有L个layer那么对于一个gt框就有L x K个postive正样本。
2. 然后计算L x K个正样本anchor与gt框的IOU，统计出均值和标注差m和v，那么由此计算出给定gt框的IOU阈值为t = m + v. 那么最后就选择IoU大于等于t的作为正样本其余作为负样本。
3. 在实际使用的时候(论文中的伪代码)限制选中的anchor box的中心点需要在gt框内部
4. 如果一个anchor框可以和多个gt框匹配那么就去IOU最大的

至于atss的解释论文给了比较多的阐述，实际也是FCOS做法的insight。最后点上面还是可以的。
