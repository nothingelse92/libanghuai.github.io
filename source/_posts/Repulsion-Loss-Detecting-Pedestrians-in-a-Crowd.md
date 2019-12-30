---
title: 'Repulsion Loss: Detecting Pedestrians in a Crowd'
date: 2019-12-08 23:02:20
tags:
  - Pedestrian Detection
  - Loss
---
URL：https://zpascal.net/cvpr2018/Wang_Repulsion_Loss_Detecting_CVPR_2018_paper.pdf

一篇解决行人检测遮挡场景的论文，切入点是loss，作者认为行人检测问题在遮挡场景一个比较显著的难点是boundingbox的**shift**问题，简言之就是由于多个人密集聚在一起的时候因为个体之间特征过于相似所以网络在预测定位的时候会产生偏移的现象，从而导致位置不够精确甚至会产生FP，论文的figure1给了一个比较简单的示意图(虚线红色框就是描述这个现象的dt框)：

![](Repulsion-Loss-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-0923.54.13.png)

OK,那就承接上面这张图直接说一下这篇论文的核心内容，repulsion Loss:

![](Repulsion-Loss-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-1000.03.37.png)

Repulsion Loss主要分成三个组成部分，L<sub>Attr</sub>, 就是一般的回归Loss比如L1、L2...之类的，毕竟为了refine定位框必要的监督信息还是需要的：

![](Repulsion-Loss-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-1000.06.10.png)

另一部分叫 L<sub>RepGT</sub>，这一项的目的是希望每一个proposal与相临近的gt框(不是当前proposal match的gt框)相远离，也就是对于第一张图，如果要预测紫色框，那么L<sub>RepGT</sub>就用来控制紫色框与蓝色框相远离从而缓解shift的问题，做法的话很直接，对于给定的proposal选择一个与之IoU最大的gt框然后计算Loss：

![](Repulsion-Loss-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-1000.14.24.png)


![](Repulsion-Loss-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-1000.14.41.png)

这里也引入了一个超参数来平滑loss：

![](Repulsion-Loss-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-1000.14.47.png)

Repulsion Loss的最后一项叫L<sub>RepBox</sub>, 其主要目的是希望对于任意match到不同gt的两个proposal之间需要尽可能的远离，然后形式上也比较类似了：

![](Repulsion-Loss-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-1000.19.12.png)

整体感觉这篇论文讲的点都还make sense但是都有点治标不治本
