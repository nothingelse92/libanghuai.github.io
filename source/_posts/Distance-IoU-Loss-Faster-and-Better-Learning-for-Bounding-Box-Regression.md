---
title: 'Distance-IoU Loss: Faster and Better Learning for Bounding Box Regression'
date: 2020-05-24 13:22:24
tags:
  - Object Detection
  - Loss
---
URL: https://arxiv.org/abs/1911.08287

AAAI 2020的一篇论文，算是对IoU系列的一个总结同时提出了新的基于IoU的Loss, DIoU和CIoU

论文上来先分析一下目前IoU系列Loss的缺点:
1. IoU : 对non-overlap的框不友好，无法处理这一类的问题
2. GIoU: GIoU是在IoU的基础上增加了最小外接矩形的惩罚, C是最小外接矩形的面积, B是预测框，B<sup>gt</sup>就是对应的gt框了, 所以解决了IoU对non-overlap框的忽略:

    ![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.26.08.png)

    GIoU的两个问题:

    a. GIoU做监督的情况下模型倾向于先出一个比较大的BoundingBox，这样两个框的non-overlap的概率会很小，然后再走IoU Loss的逻辑一步步优化，这样的问题会导致GIoU模型训练收敛速度变慢

    ![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.29.11.png)

    b. GIoU对于包含关系的两个框无法处理，因为会退化到IoU的情况，比如下图，这种情况是不合理的:

    ![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.30.22.png)

所以作者在论文里提出了DIoU来处理GIoU的缺点，解法也很简单，引入两个框中心点之间的距离来处理GIoU的缺点, b代表框的中心点，c代表最小外接矩形的对角线长度，ρ就代表欧式距离:

![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.31.32.png)

示意图:

![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.33.00.png)

在DIoU的基础上作者又提出了CIoU(Complete IoU Loss), 综合考虑两个框的Overlap/center/ratio，所以在DIoU的基础上又加上了ratio的约束:

![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.34.47.png)

v是对角度的约束，希望dt和gt框w/h比也趋于一致:

![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.35.06.png)

α 是一个trade off的超参数,原则很简单，框的回归最重要，如果两个框的overlap足够大再去处理一下ratio的问题:

![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.35.59.png)

最后再看下作者做的几组模拟实验，结果也验证了作者的一些解释:

![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.37.21.png)

![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.37.39.png)

点上面也是还可以的:

![](Distance-IoU-Loss-Faster-and-Better-Learning-for-Bounding-Box-Regression-屏幕快照 2020-05-24 下午1.38.22.png)

D代表用DIoU的计算方式取代IoU的计算方式去处理NMS问题，不过看上去这个小trick的提升很有限
