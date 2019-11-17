---
title: Object Detection based on Region Decomposition and Assembly
date: 2019-03-25 14:25:39
tags:
  - Detection
---
URL: https://arxiv.org/abs/1901.08225
AAAI2019的一篇关于检测的论文，论文主要的出发点是想解决遮挡场景下的物体检测问题，整个逻辑基于Faster RCNN的框架来做，主要思路是先把proposal分part分别来提取特征然后再通过一定的方法将其merge到一起来突出可见部分的特征，从而得到更可信的信息。

论文所提方法整体基于Faster RCNN的逻辑，具体分成两个部分MRP和RDA，下面是具体的示意图：
![](Object-Detection-based-on-Region-Decomposition-and-Assembly-图片 1.jpg)
**Multi-scale region proposal (MRP) network**
这一部分做法其实很简单，就是给RPN的输出proposal给一些scale来丰富porposal的覆盖程度，论文中用了[0.5, 0.7, 1, 1.2, 1.5]共5个scale：
![](Object-Detection-based-on-Region-Decomposition-and-Assembly-屏幕快照 2019-03-16 下午5.47.30.jpg)
**Region decomposition and assembly (RDA) network**
这一部分可以理解为整个方法的核心了，主要分成Decomposition 和 Assembly两个部分， Decomposition 部分将经过ROI Pooling之后的feature map x2然后将其等分成上下左右四部分，每一个部分都会通过conv提取依次特征然后分别merge，merge的方法实际就是element wise max，从而可以显著一些可见区域的特征。这样4个part最终还是会merge为一个feature map，megre完的结果最后再跟全图的feature map在做一次同样的操作得到最后的output，整个逻辑的话图示很清楚：
![](Object-Detection-based-on-Region-Decomposition-and-Assembly-屏幕快照 2019-03-16 下午5.52.33.png)
**实验结果**
![](Object-Detection-based-on-Region-Decomposition-and-Assembly-屏幕快照 2019-03-16 下午5.56.03.png)
