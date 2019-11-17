---
title: Mask Scoring R-CNN
date: 2019-03-19 22:54:28
tags:
  - Segmentation
---
URL:https://arxiv.org/abs/1903.00241
论文的出发点很直观，就是为了优化在目前的一些instance segmentation的方法中用classification score来标注一个mask的质量，这个其实很显然和实际应用场景是完全不一致的，比如论文中给出的例子：
![](Mask-Scoring-R-CNN-屏幕快照 2019-03-19 下午10.56.07.png)

所以为了解决这个问题，作者以mask rcnn为依托在其基础上增加了 MaskIoU 分支用来出mask和gt之间的IoU，而mask的质量分S<sub>mask</sub> = S<sub>cls</sub> * S<sub>mask_IoU</sub>：
![](Mask-Scoring-R-CNN-屏幕快照 2019-03-19 下午11.01.42.png)
具体MaskIoU分支的逻辑图例给的比较清楚，mask分支的输出通过max pooling的作用之后和RoIAlign的结果进行concat作为MaskIoU分支的输入，经过conv和fc之后得到最后的C个iou，maskiou的计算也比较简单，mask分支的输出卡一个阈值0.5就可以实现二值化，二值化后的mask可以和gt可以比较简单的计算出IoU。

针对MaskIoU分支输入的形式作者也做了好几个尝试：
![](Mask-Scoring-R-CNN-屏幕快照 2019-03-22 下午8.54.04.png)
最后显示直接相加效果是最好的：
![](Mask-Scoring-R-CNN-屏幕快照 2019-03-22 下午8.55.55.png)

最后的点：
![](Mask-Scoring-R-CNN-屏幕快照 2019-03-22 下午8.53.56.png)
