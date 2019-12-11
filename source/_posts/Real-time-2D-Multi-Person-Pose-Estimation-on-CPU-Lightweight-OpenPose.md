---
title: 'Real-time 2D Multi-Person Pose Estimation on CPU: Lightweight OpenPose'
date: 2019-01-28 16:12:08
tags:
  - Skeleton
  - Landmark
---
URL: https://arxiv.org/pdf/1811.12004.pdf
论文主要在探索把OpenPose做的轻量化一点，从模型结构到工程优化都做了不少工作，基本把一些常见的优化操作都尝试了一下，比如把原有的VGG backbone替换成针对手机端的mobilenet、把7x7的conv化成1 x 1 + 3 x 3 + 3 x 3 + residual connection、把PAF和joint两个分支进行部分合并共享参数等等。最后模型的复杂度从61.7GFlops降到9GFlops的同时AP只从43.3%降到42.8%，速度直接的量化结果（NUC支持FP16、CPU支持FP32）：
![](Real-time-2D-Multi-Person-Pose-Estimation-on-CPU-Lightweight-OpenPose-屏幕快照 2018-12-23 下午11.55.18.png)

![](Real-time-2D-Multi-Person-Pose-Estimation-on-CPU-Lightweight-OpenPose-屏幕快照 2019-01-28 下午4.16.00.png)
