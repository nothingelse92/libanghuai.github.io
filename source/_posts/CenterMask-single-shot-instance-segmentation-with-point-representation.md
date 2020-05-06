---
title: 'CenterMask: single shot instance segmentation with point representation'
date: 2020-05-06 13:06:37
tags:
  - Segmentation
---
URL: https://arxiv.org/abs/2004.04446

和上一篇论文重名了，也中了CVPR 2020，论文中作者将Instance Segmentation归结为两个问题，一个是如果区分Instance，另一个是如果得到更加精确的分割结果，作者提出的网络CenterMask相关设计也对应着这两部分:

![](CenterMask-single-shot-instance-segmentation-with-point-representation-屏幕快照 2020-05-06 下午9.23.10.png)

论文中的网络结构设计主体follow CenterNet，模型总共有5个分支:

1. Heatmap: 用来出物体的中心和类别
2. Offset: 用来refine Heatmap出的中心位置
3. Size: 用来出物体的H和W
4. Shape：利用Heatmap和Offset出的中心点对应的feature(长度是1 x 1 x S<sup>2</sup>)出粗分割的结果，这样Size + Shape就可以表示不同大小形态的物体了, 具体做法下图描述的挺清楚的:

    ![](CenterMask-single-shot-instance-segmentation-with-point-representation-屏幕快照 2020-05-06 下午9.26.37.png)

5. Saliency: Channel为1的输出，和Semantic Segmentation的作用一致，只是这里只需要区分前后背景不用区分具体类别
6. 那么最后分割的结果就是 Mask<sub>*result*</sub> = Sigmoid(Mask<sub>*Size + Shape*</sub>) * Sigmoid(Mask<sub>*Saliency*</sub>)，加上Heatmap的预测结果就可以实现Instance Segmentation了
7. 然后网络会直接监督Mask<sub>*result*</sub>，下图中的M<sub>k</sub>:

    ![](CenterMask-single-shot-instance-segmentation-with-point-representation-屏幕快照 2020-05-06 下午9.38.13.png)

这篇论文的主要内容就是这些了，loss什么的细节可以具体看论文。这篇论文看到这的话我一开始有一个疑问就是既然Size + Shape目的是区分instance，那么比如框/点这样很粗的定位信息就够了...但是对于遮挡问题分割会更加友好一点..
