---
title: Objects as Points
date: 2020-03-12 13:28:16
tags:
  - Object Detection
---
URL：https://arxiv.org/abs/1904.07850

Anchor free做检测的论文CenterNet，应该是中了CVPR2019，整体是把做Pose常用的heatmap的做法放到了检测任务上，当然也可以理解为Pose Estimation/Detection的一种统一，毕竟论文里面也是给出了同样框架下在Pose Estimation和3D Detection任务上面的表现。

具体看下在普通的检测任务上是怎么做的吧：

![](Objects-as-Points-屏幕快照 2020-03-12 下午6.27.05.png)

对问题的建模比较好理解，通常物体的框标注为(x1, y1, x2, y2)，那么论文中作者把物体框定义为Center Point + Width/Height，整个网络基于heatmap出结果，每一个pixel出C + 4维输出，所以heatmap最后输出的channel应该是(C + 4), C代表类别，C个channel每一个pixel位置的值就代表这个pixel属于这个类别的概率，剩下来的4维分别是基于这个pixel的offset(2维) 和 object的长宽(2维), 分类用的focal loss，回归用的L1 Loss，至于在inference的时候论文里写的也挺清楚的：找出C每个heatmap的峰值(最多100个)，峰值的定义就是比周围8个近邻大的点，找到这100个峰值之后接可以利用对应4个channel的输出还原出对应的100个框，这就是最后的结果，这些框不再需要走NMS，可以通过简单的卡阈值完成模型的输出。

CenterNet整体的思想就是这些，十分简单有效是一篇不错的工作，另外一个好处是这篇论文最后的代码也开源了: https://github.com/xingyizhou/CenterNet
