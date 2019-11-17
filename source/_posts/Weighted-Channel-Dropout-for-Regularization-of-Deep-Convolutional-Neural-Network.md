---
title: >-
  Weighted Channel Dropout for Regularization of Deep Convolutional Neural
  Network
date: 2018-12-14 21:01:39
tags:
  - Regularization
---

URL:http://home.ustc.edu.cn/~saihui/papers/aaai2019_weighted.pdf
**【Summary】**AAAI2019的一篇关于Regularization的论文，整体感觉可以理解为SENet思想在Regularization中的应用。论文中作者提出了Weighted Channel Dropout(WCD)的逻辑（为每一个channel计算权重、构造取舍的概率...）来对channel进行选择性的DropOut。

![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.03.34.png)
上面这张图是WCD的整个Pipeline，整个结构主要可以分成三个部分：
+ **Rating Channels**：这个模块主要是为每一个channel构造一个全权重，逻辑很简单就是Global Average Pooling：
![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.18.15.png)
+ **Weighted Random Selection (WRS)**: 这一部分逻辑其实也很简单，作者定义了每个channel被选择的概率为：
![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.20.57.png)
那么在具体计算的时候是这样做的,r<sub>i</sub>是一个（0，1）之间的随机数，对于这两个计算方式的等价性，论文中作者给出了引用论文可以具体参考：
![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.21.58.png)
![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.23.52.png)
+ **Random Number Generation (RNG)**: 这个可以理解为在WRS基础上的一个补充吧，在实际应用中有时间可用的数据量比较小，那么这种情况下通常会用pretrain的模型来初始化网络，那么作者认为这种情况下channel之间的差距会更大，可能只有少部分的channel会有比较大的响应，那么根据WRS选出来的channel可能也很像，所以针对WRS选中的channel依然会有1 - q的概率被抛弃,那么最终每一个channel的状态就和WRS、RNG都有关：
![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.30.54.png)
论文中取alpha为：
![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.31.35.png)
**一些实验结果**

![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.32.15.png)
![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.32.21.png)
作者还贴了一个和SENet比较的结果：

![](Weighted-Channel-Dropout-for-Regularization-of-Deep-Convolutional-Neural-Network-屏幕快照 2018-12-14 下午9.44.02.png)
