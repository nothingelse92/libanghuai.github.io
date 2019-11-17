---
title: Adaptive Wing Loss for Robust Face Alignment via Heatmap Regression
date: 2019-04-20 21:50:07
tags:
  - Landmark
---
URL:https://arxiv.org/abs/1904.07399
最新挂出来的关于人脸landmark的论文，可以理解为整合wing loss + look at boundary进行的优化，wing loss在cvpr2018提出的时候是直接应用在回归landmark点坐标，作者想将其应用到heatmap出点的逻辑上因而提出了adaptive wing loss，同时在网络中引入boundary的信息来辅助模型的训练，在benchmark上的表现还是很不错的，部分指标可以和wing loss、lab拉开比较大的差距。

这篇论文的核心是提出了adaptive wing loss的概念，基于heatmap出点的方法作者认为模型需要focus在两个主要的部分，一个是前景区域，另外一个是比较难的背景区域，比较难的背景区域定义为前景区域附近的背景，具体的划分论文中也给了一个简单的示例：
![](Adaptive-Wing-Loss-for-Robust-Face-Alignment-via-Heatmap-Regression-屏幕快照 2019-05-29 下午10.52.16.png)
因此作者在设计adaptive wing loss的时候也考虑了heatmap上不同pixel的重要性，那么为了更好的进行模型的训练，理想中的Loss function可以实现当训练初期gt与dt差距比较大的时候可以快速收敛，梯度可以直观的反馈gt与dt的差距，当gt和dt差距比较小的时候，前景和比较难的的背景pixel需要加大重要性，而其他的背景pixel需要削弱影响因素，所以最终adaptive wing loss具体的形式如下图，其中A = ω(1/(1 + (θ/ε)(α−y)))(α − y)((θ/ε)(α−y−1))(1/ε)，C = (θA−ω ln(1+(θ/ε)α−y))， 实际实验的时候α = 2.1,ω = 14,ε = 1,θ = 0.5
![](Adaptive-Wing-Loss-for-Robust-Face-Alignment-via-Heatmap-Regression-屏幕快照 2019-04-18 下午10.50.38.png)
几个细节需要考虑：loss的第一部分指数为α-y，这个参数就是控制了不同pixel可以表现不同的重要性，比如越接近高斯分布的中心这个指数越小，否则越大，loss的第二部分当差距比较大的时候loss是一个常数可以比较块的收敛。
另外为了突出对gt的优化，作者提出了一个weigthed loss map的概念，本质就是对loss加权：
![](Adaptive-Wing-Loss-for-Robust-Face-Alignment-via-Heatmap-Regression-屏幕快照 2019-04-18 下午11.28.31.jpg)
![](Adaptive-Wing-Loss-for-Robust-Face-Alignment-via-Heatmap-Regression-屏幕快照 2019-04-18 下午11.30.26.png)
此外作者也参考了Look at boundary论文的做法将landmark组成的轮廓引入到网络中，感觉像是辅助监督，论文中是说了用coorconv来encode边缘的信息，具体细节论文也没有给出：
![](Adaptive-Wing-Loss-for-Robust-Face-Alignment-via-Heatmap-Regression-屏幕快照 2019-04-18 下午8.36.11.png)
实验结果还是很高的：
![](Adaptive-Wing-Loss-for-Robust-Face-Alignment-via-Heatmap-Regression-屏幕快照 2019-04-18 下午11.36.21.png)
![](Adaptive-Wing-Loss-for-Robust-Face-Alignment-via-Heatmap-Regression-屏幕快照 2019-04-18 下午11.36.27.png)
作者对基于heatmap出点的方式所进行的loss设计的思考还是很有参考意义的，最后做出来的点也很高，感觉可以尝试
