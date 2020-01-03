---
title: >-
  Generalized Intersection over Union: A Metric and A Loss for Bounding Box
  Regression
date: 2019-03-03 10:43:07
tags:
  - Detection
  - Loss
---
URL：https://arxiv.org/abs/1902.09630
这是CVPR2019的一篇论文。本论文主要提出了GIoU的概念来优化IoU在评估或者IoU Loss在训练中的一些问题
这是利用Ln-Loss来优化bbox回归问题的常见bug，不同的overlap程度在Ln-loss看来都一样，但是实际上对于IoU或者GIoU确实不一样的，很显然后者更合理：

![](Generalized-Intersection-over-Union-A-Metric-and-A-Loss-for-Bounding-Box-Regression-屏幕快照 2019-03-03 上午11.01.57.png)

然后就是IoU的不足，第一是对于IoU为0的情况也就是两个box没有交集的情况无法处理，IoU Loss在这种情况下没有梯度的回传。第二就是对于IoU的评价指标对于overlap的方式没有什么体现，比如下图中的示例，IoU都是0.33，但是它们的链接方法是不一样的，或者说对于box回归的任务来说我们的接受度也是不一样的，很显然下图是依次递减，恰好GIoU刚好可以做到这一点：
![](Generalized-Intersection-over-Union-A-Metric-and-A-Loss-for-Bounding-Box-Regression-屏幕快照 2019-03-03 上午11.05.36.png)
接下来就是GIoU的计算，C\(AUB)可以理解为两框在convex之内的空白部分了,GIoU计算的方法主要claim一点，更整齐的overlap方法会导致空白部分很小所以GIoU更大：
![](Generalized-Intersection-over-Union-A-Metric-and-A-Loss-for-Bounding-Box-Regression-屏幕快照 2019-03-03 上午11.08.11.png)
GIoU Loss：
![](Generalized-Intersection-over-Union-A-Metric-and-A-Loss-for-Bounding-Box-Regression-屏幕快照 2019-03-03 上午11.09.13.png)
不过从结果上来看，GIoU似乎只对YoloV3这样anchor相对比较稀疏的模型比较有效，也算比较符合GIoU的定义吧：
![](Generalized-Intersection-over-Union-A-Metric-and-A-Loss-for-Bounding-Box-Regression-屏幕快照 2019-03-03 上午11.11.37.png)
![](Generalized-Intersection-over-Union-A-Metric-and-A-Loss-for-Bounding-Box-Regression-屏幕快照 2019-03-03 上午11.11.42.png)
![](Generalized-Intersection-over-Union-A-Metric-and-A-Loss-for-Bounding-Box-Regression-屏幕快照 2019-03-03 上午11.11.50.png)
