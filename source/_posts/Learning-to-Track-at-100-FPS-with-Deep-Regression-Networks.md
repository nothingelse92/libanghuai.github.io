---
title: Learning to Track at 100 FPS with Deep Regression Networks
date: 2019-01-14 22:24:35
tags:
  - Track
  - Detection
---
URL:https://arxiv.org/abs/1604.01802
论文主要提出了GOTURN的tracking框架，整体还是比较naive的一些tracking逻辑，下图是GOTURN整个的pipeline:
![](Learning-to-Track-at-100-FPS-with-Deep-Regression-Networks-屏幕快照 2019-01-14 下午10.26.02.png)

GOTURN整个框架是针对连续的两帧来做的，整个框架因此也主要分成两个分支，分别针对当前帧和前一帧，给定当前帧(t)和前一帧(t-1),t-1帧的物体是已知的，比如物体框中心为center(c<sub>x</sub>, c<sub>y</sub>),长宽分别是w、h。那么对于下面一个分支首先会以center为中心，长宽分别是k1w, k1h去抠图，k1这个系数用于控制context信息的量。那么对于上面一个分支，会从同样的位置center作为中心，长宽分别是k2w,k2h去抠图作为当前帧的搜索区域。论文中也提及了一下通常k2 = 2。这两个分支的输出最后会被concat到一起送入一个3层的FC网络，每层FC有4096个神经元，每个分支本身的结构是CaffeNet的前五层。

那么在具体训练的时候，论文中主要提及了一些数据增强的细节，比如论文就直接说明GOTURN本身在训练的时候就是针对低速运动的物体进行的设计，对于高速运动的物体并不能cover。至于GOTURN对低速运行物体的跟踪也跟论文中提到的数据增强方法有关，模型在具体训练的时候会构造这样的pair对：
![](Learning-to-Track-at-100-FPS-with-Deep-Regression-Networks-屏幕快照 2019-01-14 下午10.43.16.png)
![](Learning-to-Track-at-100-FPS-with-Deep-Regression-Networks-屏幕快照 2019-01-14 下午10.43.23.png)
其中控制偏移幅度的变量delta服从Laplace分布。

**具体的实验结果**
![](Learning-to-Track-at-100-FPS-with-Deep-Regression-Networks-屏幕快照 2019-01-14 下午10.46.49.png)
Ablation Study:
![](Learning-to-Track-at-100-FPS-with-Deep-Regression-Networks-屏幕快照 2019-01-14 下午10.47.31.png)
