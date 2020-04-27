---
title: Large-Margin Softmax Loss for Convolutional Neural Networks
date: 2020-04-19 19:49:41
tags:
  - Loss
  - Classification
---
URL: http://arxiv.org/abs/1612.02295

ICLR2016的一篇论文，出发点挺直接的一篇论文，主要在优化Softmax Loss，提出了Large-Margin Softmax Loss来促使网络学到更加有区分度的Feature.

首先来分解一下Softmax Loss的计算:

![](Large-Margin-Softmax-Loss-for-Convolutional-Neural-Networks-屏幕快照 2020-04-19 下午7.52.31.png)

这里的f就是最后一层FC的输出： f<sub>yi</sub> = W<sub>yi</sub><sup>T</sup> * x<sub>i</sub>, x<sub>i</sub>代表第i张图抽取出来的feature，W<sub>yi</sub>就是最后一层FC连接到前一层feature的weights，**其实可以理解为y<sub>i</sub>类在高维空间的类中心**. 又因为向量的内积可以直接转化成cosine运算:

W<sub>yi</sub><sup>T</sup> * x<sub>i</sub> = ||W<sub>j</sub>|| ||x<sub>i</sub>|| cos(θ<sub>j</sub>), θ<sub>j</sub>当然就是这两个向量之间的夹角了. 所以Softmax Loss又可以转化成:

![](Large-Margin-Softmax-Loss-for-Convolutional-Neural-Networks-屏幕快照 2020-04-19 下午7.59.03.png)

那么Large-Margin Softmax Loss提出了和SVM类似的分类平面的问题，它希望inter-class之间的margin可以大一些，所以在Softmax Loss的基础上引入一个变量m来控制分类平面的margin.

在原始的Softmax Loss中针对一个二分类问题，假设样本x属于类别1，那么我们希望||W<sub>1</sub>|| ||x|| cos(θ<sub>1</sub>) > ||W<sub>2</sub>|| ||x|| cos(θ<sub>2</sub>), (cosine在[0,π]区间内是一个单调递减的函数，同时指数函数还是一个单调递增的函数)，那么在Large-Margin Softmax Loss上就转化成||W<sub>1</sub>|| ||x|| cos(mθ<sub>1</sub>) > ||W<sub>2</sub>|| ||x|| cos(θ<sub>2</sub>)，所以Large-Margin Softmax Loss：

![](Large-Margin-Softmax-Loss-for-Convolutional-Neural-Networks-屏幕快照 2020-04-19 下午8.06.10.png)

其中:

![](Large-Margin-Softmax-Loss-for-Convolutional-Neural-Networks-屏幕快照 2020-04-19 下午8.06.33.png)

同时满足D(π/m) = cos(π/m). 论文里面取:

![](Large-Margin-Softmax-Loss-for-Convolutional-Neural-Networks-屏幕快照 2020-04-19 下午8.07.44.png)

论文里给的一张示意图挺能说明Large-Margin Softmax Loss的insight的：

![](Large-Margin-Softmax-Loss-for-Convolutional-Neural-Networks-屏幕快照 2020-04-19 下午8.08.13.png)

效果上面当然也挺好了:

![](Large-Margin-Softmax-Loss-for-Convolutional-Neural-Networks-屏幕快照 2020-04-19 下午8.09.06.png)
