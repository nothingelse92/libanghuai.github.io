---
title: Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields
date: 2018-12-24 13:27:13
tags:
  - Pose
  - BottomUp
  - Detection
---
URL:
论文所提的模型结构整体是一个bottom-up的结构，F是通过backbone网络（论文中用的是vgg）提取的特征，整个结构分成两个分支，一个分支用来预测joint的heatmap，另一个分支可以理解为用来预测joint之间的联系（Part Affinity Field），实际上是joint对应的向量：
