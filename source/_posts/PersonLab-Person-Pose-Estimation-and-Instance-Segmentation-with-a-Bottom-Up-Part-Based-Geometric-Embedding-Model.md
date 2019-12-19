---
title: >-
  PersonLab: Person Pose Estimation and Instance Segmentation with a Bottom-Up,
  Part-Based, Geometric Embedding Model
date: 2019-01-24 17:54:05
tags:
  - BottomUp
  - Skeleton
  - Landmark
---
URL: https://arxiv.org/abs/1803.08225
这篇论文其实和之前读的G-RMI那篇论文是同一个作者，G-RMI是top down的逻辑，而这篇论文是bottom up的逻辑。论文所提方法同时在做pose estimation和instance-level person segmentation两个task。pose estimation主要是通过预测点对之间的向量来做group，seg则是主要借鉴embedding的逻辑通过设计新的loss函数来优化效果。
下图是论文所提方法的整个pipeline，两个大的分支，pose + seg：
![](PersonLab-Person-Pose-Estimation-and-Instance-Segmentation-with-a-Bottom-Up-Part-Based-Geometric-Embedding-Model-屏幕快照 2019-01-03 下午11.18.23.jpg)

先说一下论文中对pose estimation的做法，这一部分主要涉及三个内容：
+ **heatmap生成**：k个joint， k channel的heatmap，同时针对每一个joint人为围绕这个joint设置一个半径为R的圆（论文中R=32pixel），圆内的点为1，其余为0，构造一个分类任务来监督heatmap的生成。
+ **short-range offset**: 把G-RMI那篇论文中用到的方法拿过来应用，主要是在heatmap的基础上还会预测在joint半径R pixel内的点与当前joint点的offset。然后再整合heatmap和offset（hough voting）得到最后的位置信息具体如上图。
+ **mid-range offset**: 这个内容主要是用来对多人做group的。主要是想通过预测两个joint之间的offset向量在inference的进行instance的划分，但是在整张图中预测两点之间准确的offset是很难的，所以作者就把这个问题转换成两个joint半径R区域内相对应的点之间的offset和圆内点到joint之间的offset的加和来避免这样的问题，相当于用两个相对比较糙的结果来refine 最后的offset：
![](PersonLab-Person-Pose-Estimation-and-Instance-Segmentation-with-a-Bottom-Up-Part-Based-Geometric-Embedding-Model-屏幕快照 2019-01-03 下午11.20.40.png)
论文中另外一个主要的部分就是关于seg的内容, 也引出了论文提到的第三个offset，long-range offset：
![](PersonLab-Person-Pose-Estimation-and-Instance-Segmentation-with-a-Bottom-Up-Part-Based-Geometric-Embedding-Model-屏幕快照 2019-01-06 下午11.19.49.jpg)
论文所提做seg的方法主要是借鉴了embedding 的逻辑比如associate embedding方法，这些方法的一个主要思想就是设计一个loss使得属于同一个instance的点离的尽量近，不属于同一个instance的点离的尽量远，long-range offset就是作者在设计loss函数（距离函数）时提到的概念。long-range offset就是instance内的点到某一个joint的offset，上图画的比较清楚，具体计算distance的时候是：
![](PersonLab-Person-Pose-Estimation-and-Instance-Segmentation-with-a-Bottom-Up-Part-Based-Geometric-Embedding-Model-屏幕快照 2019-01-06 下午11.30.27.png)
其中G(x) = x + L(x), L(x）就是long-range offset.

## Experiments
Pose Estimation:
![](PersonLab-Person-Pose-Estimation-and-Instance-Segmentation-with-a-Bottom-Up-Part-Based-Geometric-Embedding-Model-屏幕快照 2019-01-06 下午11.32.43.png)
Segmentation:
![](PersonLab-Person-Pose-Estimation-and-Instance-Segmentation-with-a-Bottom-Up-Part-Based-Geometric-Embedding-Model-屏幕快照 2019-01-06 下午11.33.19.png)
