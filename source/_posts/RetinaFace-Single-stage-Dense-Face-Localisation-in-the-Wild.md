---
title: 'RetinaFace: Single-stage Dense Face Localisation in the Wild'
date: 2019-12-16 22:29:19
tags:
  - Multi-Task
  - Landmark
  - Face
---
URL:https://arxiv.org/abs/1905.00641

这篇论文就很糙快猛了，multitask(人脸检测+人脸关键点定位+人脸3D Mesh + 人脸分类)涨点：
![](RetinaFace-Single-stage-Dense-Face-Localisation-in-the-Wild-屏幕快照 2019-05-05 下午5.45.42.jpg)
Loss也比较直接，Lcls为softmax loss，Lbox和Lpts为L1 Loss，Lpixel具体如下，R为3D图片在2D平面的投影，I为gt：
![](RetinaFace-Single-stage-Dense-Face-Localisation-in-the-Wild-截屏2019-12-1622.39.25.png)
其他这篇论文似乎就没有什么可以描述的了...
