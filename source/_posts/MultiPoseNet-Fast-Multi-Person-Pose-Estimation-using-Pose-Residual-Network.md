---
title: 'MultiPoseNet: Fast Multi-Person Pose Estimation using Pose Residual Network'
date: 2018-12-24 12:40:27
tags:
  - BottomUp
  - Pose
  - Landmark
---
URL: https://arxiv.org/abs/1807.04067
ECCV2018一篇利用bottom up方法做pose estimation的论文。论文所提出的方法主要是分别利用两个分支一个分支用来检测人体框，另一个分支用来出关键点的heatmap。然后再利用一个网络来merge 两个分支出的结果，将关键点分别映射到对应的人体框上实现多人的姿态估计。

下图是本文所提方法的大体Pipeline，整个pipeline可以分成三个部分，人体关键点检测、人体框检测和关键点的聚类（PRN）：
![](MultiPoseNet-Fast-Multi-Person-Pose-Estimation-using-Pose-Residual-Network-b80970b3e1b91db9992796fdada5d5cacddc1531.jpg)
+ **Backbone**：Resnet + FPN
+ **关键点检测**：整个分支结构如下图，比较直观，L2 loss，总共出K + 1 个heatmap，+1为seg的结果：
![](MultiPoseNet-Fast-Multi-Person-Pose-Estimation-using-Pose-Residual-Network-a42ffc7c2d336f7a095fc898d260ef90c728a7f6.png)
+ **人体框检测**：就是直接上的RetinaNet
![](MultiPoseNet-Fast-Multi-Person-Pose-Estimation-using-Pose-Residual-Network-96f37d11a64105bce2440ed6b6955231dd1a6949.png)
+ **Pose Residual Network (PRN)**：这一部分内容是论文的核心，主要是利用人体框将关键点检测的结果映射到对应的instance上。具体做法是在关键点检测的结果上利用对应人体框的位置crop出一样大小的patch，这样就可以得到K x W x H大小的feature map，K为关键点个数，W、H为对应人体框的size，然后把这个feature map resize到固定大小(36 x 56)作为PRN的输入。那么对于上图的c、d这种同一个框有多个人关键点overlap的情况，主要是利用一个多层感知机（residual multilayer perceptron）来把人体框对应instance的关键点提取出来，具体逻辑如下图：
![](MultiPoseNet-Fast-Multi-Person-Pose-Estimation-using-Pose-Residual-Network-c340c5ea72c9c75d031dbcba3c8b569d4e393023.jpg)
**结果：**
![](MultiPoseNet-Fast-Multi-Person-Pose-Estimation-using-Pose-Residual-Network-9255a515da09097264bb0c17a00cd420a3a16291.png)
![](MultiPoseNet-Fast-Multi-Person-Pose-Estimation-using-Pose-Residual-Network-d87795e947cb3e80cf5311605ec05df40db408f6.png)

这篇论文另一个比较重要的点是虽然整个网络的pipeline比较多但是inference的速度还是比较快的，对于典型的COCO图片（～3人）可以达到23FPS的速度，用来merge的PRN网络输入比较小层数也不深。
