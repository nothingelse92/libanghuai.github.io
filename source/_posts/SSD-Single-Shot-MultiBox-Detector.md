---
title: 'SSD: Single Shot MultiBox Detector'
date: 2019-02-03 22:09:53
tags:
  - Object Detection
  - Classic
---
URL: https://arxiv.org/pdf/1512.02325.pdf
Code: https://github.com/weiliu89/caffe/tree/ssd
**重读经典系列第一篇：SSD**
SSD是很经典的Single Stage检测网络，至今仍有很多的工作是基于SSD在改进。
不同于典型的Two stage检测网络将proposal的生成放在RPN网络来做，然后后续的网络branch基于RPN的结果进行Refine，SSD将Proposal直接放到网络后面的feature map上来做，基本逻辑如下图，实际和FasterRCNN网络生成anchor的逻辑是一模一样的：
![](SSD-Single-Shot-MultiBox-Detector-屏幕快照 2019-02-03 下午10.33.52.png)

至于proposal具体的scale，论文中也是给出具体的公式的，整体其实就是给定最大和最小scale(s<sub>min</sub>, s<sub>max</sub>)，中间的feature map层均匀变化：
![](SSD-Single-Shot-MultiBox-Detector-屏幕快照 2019-02-03 下午10.38.39.png)
k就是具体的层，m是全部产生proposal的层
SSD网络整体的结构目前看来也比较直接，相比较传统的VGG等网络，SSD考虑到multi scale的情况，所以在VGG16 backbone的基础上又引出了N层feature map，每一层feature map的感受野都不一样，作者通过这种方式来实现multi scale的检测，每一个feature map都会出4个offset和一个分类器，回归分支的结果是用中心点坐标（x，y） + w和h来表示一个框：
![](SSD-Single-Shot-MultiBox-Detector-屏幕快照 2019-02-03 下午10.39.31.png)
训练用到的loss，对于回归用的smooth l1:
![](SSD-Single-Shot-MultiBox-Detector-屏幕快照 2019-02-03 下午10.43.17.png)
![](SSD-Single-Shot-MultiBox-Detector-屏幕快照 2019-02-03 下午10.43.21.png)
一些benchmark上的结果：
![](SSD-Single-Shot-MultiBox-Detector-屏幕快照 2019-02-03 下午10.42.36.png)
