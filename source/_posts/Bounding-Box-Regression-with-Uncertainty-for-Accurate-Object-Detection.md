---
title: Bounding Box Regression with Uncertainty for Accurate Object Detection
date: 2020-05-09 15:04:36
tags:
  - Object Detection
---
URL:https://arxiv.org/abs/1809.08545

CVPR 2019的论文，论文核心就是引入KL Loss，用分布对BoundingBox进行建模，同时利用分布学习到的variance去辅助NMS阶段对BoundingBox框的修正，挺不错的一篇论文。

首先来说它的motivation，目前检测任务的标注通常都是希望把物体的边界框标的越精准越好，但是实际情况下避免不了会引入很多的标注误差，因为有很多的case物体的边界框都很模棱两可，如下图，所以作者就想到用分布来建模BoundingBox，这样做的好处是对边界框不会那么敏感同时学习到的variance可以用于后续的后处理

![](Bounding-Box-Regression-with-Uncertainty-for-Accurate-Object-Detection-屏幕快照 2020-05-09 下午3.07.34.png)

简化起见，作者就用高斯分布来建模框, 那么就涉及到2组变量x 和 σ，x代表(x1, y1, x2, y2)这样一组边界坐标，σ用来表征定位精度（σ同样是4维对应前面提到的(x1, y1, x2, y2)）的certainty，σ = 0就代表模型觉得当前这个x预测值足够的准:

![](Bounding-Box-Regression-with-Uncertainty-for-Accurate-Object-Detection-屏幕快照 2020-05-09 下午3.10.07.png)

那么在模型结构上就可以很简单的用FC出这4维的σ了, (x1, y1, x2, y2)还是用原来的回归分支出:

![](Bounding-Box-Regression-with-Uncertainty-for-Accurate-Object-Detection-屏幕快照 2020-05-09 下午3.10.13.png)

那么既然用分布来建模BoundingBox那么比较直接的想法就是用KL Loss来监督这个分布预测，最后通过一系列的转化本文用的回归Loss就是下图了，至于如何转换就可以直接参考原文了比较简单:

![](Bounding-Box-Regression-with-Uncertainty-for-Accurate-Object-Detection-屏幕快照 2020-05-09 下午3.15.56.png)

那么这样的模型输出之后其实就可以出结果了，因为回归分支还是有4维的输出的，同时也会被KL Loss监督，论文中作者又利用高斯分布出的variance在NMS过程中Refine模型最终出的框, 伪代码如下:

![](Bounding-Box-Regression-with-Uncertainty-for-Accurate-Object-Detection-屏幕快照 2020-05-09 下午3.17.53.png)

上图伪代码中标绿的地方对应下图的具体公式:

![](Bounding-Box-Regression-with-Uncertainty-for-Accurate-Object-Detection-屏幕快照 2020-05-09 下午3.18.28.png)

我们可以先看伪代码：
1. idx ← IoU(b<sub>m</sub>, B) > 0 =>这一行最后的idx实际上就是与b<sub>m</sub>有交集的"邻居"框(IoU>0), 所以idx是一个集合
2. p ← exp(−(1 − IoU(b<sub>m</sub>, B[idx]))2/σ<sub>t</sub>) => 这一行最后的p实际上就是上述"邻居"的对应的权重，σ<sub>t</sub>没有特殊的含义就是一个可以随时调节的变量，那么这个计算公式也比较直观，IoU越大的“邻居”p越大.
3. b<sub>m</sub> ← p(B[idx]/C[idx])/p(1/C[idx]) => 这一行的C就是variance的集合，所以实际就是又利用“邻居”预测框的variance来加权框，那么很显然C越大对当前框坐标x的影响越小。

最后可以看一下各个component具体的影响:

![](Bounding-Box-Regression-with-Uncertainty-for-Accurate-Object-Detection-屏幕快照 2020-05-09 下午3.25.28.png)
