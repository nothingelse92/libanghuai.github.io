---
title: >-
  Lightweight Real-time Makeup Try-on in Mobile Browsers with Tiny CNN Models
  for Facial Tracking
date: 2019-06-15 11:09:02
tags:
  - Landmark
---
URL:https://arxiv.org/abs/1906.02260
一篇主要做轻量级Landmark应用的论文，整体novelty有限，主要是在MobileNetV2的基础上实现了一个轻量级的Facial Landmark模型，在iPhone XR上可以实现20ms的inference速度

论文中所用的模型结构整体是一个Two Stage的逻辑， 出发点其实和之前看的一篇landmark machine论文很像，第一阶段出一个比较糙的heatmap来标识landmark点大概的位置，第二阶段再基于第一阶段的结果crop出小的patch继续refine：
![](Lightweight-Real-time-Makeup-Try-on-in-Mobile-Browsers-with-Tiny-CNN-Models-for-Facial-Tracking-屏幕快照 2019-06-14 下午12.55.35.png)

几个需要明确的点：
1. 模型的主体主要参考MobileNetV2，大量使用Inverted Residual Block
2. ROI Crop主要是参考Mask RCNN用ROI Align取代ROI Pooling从而可以把不同的crop出来的patch concat到一起
3. 最后通过一组Group Conv得到第二阶段相对于第一阶段的offset heatmap，两个阶段最后的输出加和就是最后的结果了

另外需要提及的就是Loss的计算，论文没有用标注的heatmap之间计算loss的方式，而是根据每一个heatmap上的分布计算出具体的(x,y)值，最后直接监督最后的坐标：
![](Lightweight-Real-time-Makeup-Try-on-in-Mobile-Browsers-with-Tiny-CNN-Models-for-Facial-Tracking-屏幕快照 2019-06-20 上午11.26.39.png)
结果方面：
下表算是简单的ablation study：
![](Lightweight-Real-time-Makeup-Try-on-in-Mobile-Browsers-with-Tiny-CNN-Models-for-Facial-Tracking-屏幕快照 2019-06-20 上午11.27.00.png)
下表是和LAB的比较，整体还是有差距的：
![](Lightweight-Real-time-Makeup-Try-on-in-Mobile-Browsers-with-Tiny-CNN-Models-for-Facial-Tracking-屏幕快照 2019-06-20 上午11.27.27.png)
最后是速度的测试：
![](Lightweight-Real-time-Makeup-Try-on-in-Mobile-Browsers-with-Tiny-CNN-Models-for-Facial-Tracking-屏幕快照 2019-06-20 上午11.27.49.png)
