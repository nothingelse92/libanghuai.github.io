---
title: 'SNIPER: Efficient Multi-Scale Training'
date: 2018-10-22 22:43:00
tags:
  - Object Detection
---
URL: https://arxiv.org/abs/1805.09300 Github: https://github.com/MahyarNajibi/SNIPER
**【Summary】**算是在SNIP版本上的改进吧，主要想优化multi scale训练的效率问题。论文提出了chip的概念，其实就是原图中的某一块区域，在本论文中chip的大小就是512 x 512的矩形方块。网络的输入不再是原图，而是这些生成的chip。SNIPER的主要内容就是这些chip的生成逻辑。

作为网络的输入，chip也分为postive chip和negative chip, 下图是postive chip的生成示例。对于给定的一张图，会有512 x 512这样大小的chip框在原图上滑动，间隔为32个pixel。包含valid gt（valid或者invalid的定义和SNIP那篇论文逻辑一样）的chip被称为postive chip。由于对于给定的一张图片不会把每一个chip都送给网络去训练，所以通常都是贪心的选择包含valid gt最多的chip作为这张图的postive chip：

![](SNIPER-Efficient-Multi-Scale-Training-2feaec373ba84ada0c26428b063370d8588e35d8_1_690x386.jpg)
下面这张图则是negative chip的生成示例，论文中作者生成的negative chip是想包含那些比较难解的FP，所以作者用了一个简单的RPN网络来生成negative chip，下图的红点代表一个proposal（已经移除掉被postive chip包含的那些了）。negative chip被定义为至少含M个proposal的那些chip(论文中M貌似没给出具体的值)：

![](SNIPER-Efficient-Multi-Scale-Training-54af81d1f0fbbac701cb898fe22e2ee48d188efe_1_690x341.png)
贴一张实验结果：

![](SNIPER-Efficient-Multi-Scale-Training-6bcbdd2062f585821204274dfe18fd845f2a4544_1_690x316.png)

感觉这篇论文的重点用论文中的一句话可以很好的概括"it implies that very large context during training is not important for training high-performance detectors but sampling regions containing hard negatives is"
