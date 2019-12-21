---
title: Improving Object Detection With One Line of Code
date: 2019-07-01 21:41:29
tags:
  - Object Detection
  - NMS
---
URL: https://arxiv.org/pdf/1704.04503.pdf
Source: https://github.com/bharatsingh430/soft-nms

同样是一篇关于改进NMS的论文，不同于Learning non-maximum suppression，这篇论文主要分析了传统NMS算法的弊端，对其处理的逻辑进行了优化，并且号称只修改One Line of Code。传统的NMS本质是一个贪心的算法，首先选择一个confidence最高的bbox，然后剔除掉和它IoU比较大的bbox，循环这个步骤得到最后的bbox集合，那么它很明显的一个弊端就是下图的这种情况，利用传统的NMS被绿色框标记的有可能被抑制从而产生一个miss(IoU > th)。如果把IoU阈值提高又有可能增加FP，因此论文针对目前的NMS弊端提出了Soft-NMS算法。Soft-nms的整个算法伪代码在下图的右侧，在原有NMS算法的基础上只进行了小幅度的改动，从复杂度上面看也几乎没有变化。

 ![](Improving-Object-Detection-With-One-Line-of-Code-屏幕快照 2019-12-19 下午9.47.59.png)

 Soft-NMS：对于传统的NMS算法，对bbox的抑制可以理解为将bbox的score置为0，将bbox从候选框中完全剔除掉，那么作者为了缓解上图那样的问题提出soft-nms方法，将被抑制bbox的score设置为一个和IoU值相关的衰减函数，具体如下，传统的NMS算法则可以理解成soft-nms的特例，但考虑到上述的算分逻辑是不连续的，分数的突然抖动会对bbox的序列影响较大，因此作者follow相同的抑制逻辑，利用第二个指数形式的公式对bbox分数进行转化，同样满足离当前bbox越远的bbox分数影响越小，反之越大。

![](Improving-Object-Detection-With-One-Line-of-Code-屏幕快照 2019-12-19 下午9.49.33.png)

Soft NMS直观的理解：对于传统的NMS，两个Object的框如果IoU比较大的话其中一个框有可能会被干掉，那么Soft NMS为了缓解这个问题会把其中一个分数较低的Object框分降低，这样它匹配的优先级会降低，在多轮迭代之后有可能会被保留下来，从而可以整体提高recall.

当然了SoftNMS本身的做法有点治标不治本，首先框没有明确的ID信息，这种做法同样有可能保留FP，另外对于这种Overlap也没有本质的解决
