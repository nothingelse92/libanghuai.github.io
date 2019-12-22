---
title: Deep Convolutional Network Cascade for Facial Point Detection
date: 2019-09-11 23:16:31
tags:
  - Landmark
  - Cascade
---
URL: http://openaccess.thecvf.com/content_cvpr_2013/papers/Sun_Deep_Convolutional_Network_2013_CVPR_paper.pdf

关于cascade landmark localization的一篇论文，是一篇比较老的CVPR论文，论文主要提出cascade的网络结构来更准的定位landmark位置

1. Cascade结构：论文提出了三级的cascade结构，每一级网络都可以理解为是component-wise的设计

   a) 第一级网络：这一级网络分成三个网路分支，第一个网络分支以整张脸为输入，回归全部的5个landmark点；第二个网络分支以脸的上半部为输入，回归两个眼睛中心点和鼻尖的位置共3个landmark点；第三个网络分支以脸的下半部为输入，回归鼻尖和两个嘴角的位置共3个landmark点；每个landmark的最终位置为三个网络的输出的均值；

   b) 第二级和第三级网络：这两级网络的作用是一样的，每一级都有多路网络分支，每一个网络分支都只回归一个点的位置，每一个landmark点有两个网络分支去回归，两者的均值作为最终的结果，因此第二级和第三级网络分别都有10个网络分支，他们的输入都是基于第一级网络的输出landmark，从对应的landmark位置以一定的大小比例crop出输入，第三级比第二级的size更小。这两级网络学习的是相对位置的offset。

   ![](Deep-Convolutional-Network-Cascade-for-Facial-Point-Detection-thumbnail_image002.png)

   这是第一级网络分支的基本结构：

   ![](Deep-Convolutional-Network-Cascade-for-Facial-Point-Detection-thumbnail_image003.png)
   最终landmark的位置通过计算得到：

   ![](Deep-Convolutional-Network-Cascade-for-Facial-Point-Detection-thumbnail_image004.png)
