---
title: 'LSTD: A low-shot transfer detector for object detection'
date: 2020-04-24 12:51:44
tags:
  - Object Detection
  - Few Shot
---
URL: https://arxiv.org/abs/1803.01529

AAAI 2018的一篇论文，应该是最早做few shot for detection的论文，也是目前few shot detection论文都会引用对比的一篇论文，论文的一些想法还是比较make sense的。

![](LSTD-A-low-shot-transfer-detector-for-object-detection-屏幕快照 2020-04-25 下午3.30.19.png)

这张图基本涵盖LSTD所有的内容了，首先从结构上来看下LSTD是怎么做的，LSTD把分类回归分成了两步，第一步做回归，分类信息只涉及是否是物体这样的二分类，然后在第二步对第一步的回归框进行细分类别而不再做回归的refine了，
1. 第一步的回归用的SSD，作者给出了原因:
    1. **SSD很显式的在做multi scale，对于小样本训练数据来说可以很好的缓解multi scale的问题**
    2. **Faster RCNN最后回归的时候是N个分类器，所以它所有的分类都是类别相关的，但是SSD不是，SSD都是类别无关的相互share的，所以呢这种情况下在source domain下训练好的模型可以直接拿来做为target domain下模型的初始化**

2. 第二步最后的分类回归，主要是将Faster RCNN最后的FC层换成Conv层来实现分类，**好处是减少了模型参数，一定程度上缓解了过拟合的情况**。

在LSTD的基础上为了更好的进行transfer learning作者还提出了两个regulazation的方法, 这两个方法主要在target domain fine tune的时候使用:
1. 第一个是背景抑制，这样做的好处是消除掉背景的干扰让模型专注的去处理目标信息:
做法很简单，用gt框生成一个mask拍到conv feature map上得到背景的特征F<sub>BD</sub>. L<sub>BD</sub> = ||F<sub>BD</sub>||<sub>2</sub>
效果看上去还是比较明显的:

    ![](LSTD-A-low-shot-transfer-detector-for-object-detection-屏幕快照 2020-04-25 下午4.02.29.png)

2. 第二个是source domain到target domain的知识迁移，出发点是source domain的信息和target domain的信息很多情况下是互通的，比如牛马羊之类的都是有一定的相似度的，所以在target domain训练数据比较缺失的情况下借助source domain的知识辅助进行学习是很有必要的。那么做法呢也很直接:

      首先我们有两个LSTD模型: LSTD<sub>source</sub>，这个是利用全量的source domain的数据训练的. 另一个LSTD<sub>target</sub>,它是用LSTD<sub>source</sub>进行初始化的，当然最后的分类层是随机初始化的，在一般的分类层之外还会加一个额外的分类层来把LSTD<sub>source</sub>的知识迁移过来，具体看上图很好理解。然后在学习的时候对于一张输入训练图会分别送入到LSTD<sub>source</sub>和LSTD<sub>target</sub>，得到的分类logits进行CE监督，这样就显式的将source domain的信息来监督target domain的学习:

      ![](LSTD-A-low-shot-transfer-detector-for-object-detection-屏幕快照 2020-04-25 下午4.10.22.png)

伪代码:

![](LSTD-A-low-shot-transfer-detector-for-object-detection-屏幕快照 2020-04-25 下午4.14.32.png)

那么LSTD的全部就是这些内容了核心呢是降低模型学习的难度毕竟少量数据下很容易过拟合。
