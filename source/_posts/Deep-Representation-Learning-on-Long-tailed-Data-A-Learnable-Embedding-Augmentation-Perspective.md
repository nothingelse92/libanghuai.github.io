---
title: >-
  Deep Representation Learning on Long-tailed Data: A Learnable Embedding
  Augmentation Perspective
date: 2020-05-24 15:09:14
tags:
  - Classification
  - Metric Learning
  - Long Tail
---
URL: https://arxiv.org/abs/2002.10826

CVPR2020 Metric Learning 解决Long Tail的问题，motivation来自head class和tail class在高维空间的分布统计:

![](Deep-Representation-Learning-on-Long-tailed-Data-A-Learnable-Embedding-Augmentation-Perspective-屏幕快照 2020-05-24 下午3.32.10.png)

这一张图中:
1. (a) DukeMTMC-reID数据集中选择top 8的head class，可以看到intra-class和inter-class差距都比较明显，所以在分类的时候会比较容易的区分开来，这也就是head class表现比较好的根本原因.
2. (b) (a)中的8的head class随机取3个class缩减他们的样本数量形成一个head + tail class的区分，可以发现tail class的样本都挤在一个很窄的范围内，intra-class diversity很小，因为本身样本很少所以可以学习的区分度当然也很小，同样也因为样本比较少比较难拉开和其他样本的差异.
3. (c\) 加上论文中提出来的feature cloud达到的效果

论文的想法，把head class的分布(variance)迁移到tail class从而实现intra-class diversity能变大一点:

![](Deep-Representation-Learning-on-Long-tailed-Data-A-Learnable-Embedding-Augmentation-Perspective-屏幕快照 2020-05-24 下午3.38.13.png)

看一下具体做法:
1. **Intra-class Angular Distribution**： 因为我们需要做的是增加tail class的intra-class diversity, 所以需要定义一下intra-class diversity(用类别i的所有样本到类别中心的角度分布的variance来衡量intra-class diversity):

    ![](Deep-Representation-Learning-on-Long-tailed-Data-A-Learnable-Embedding-Augmentation-Perspective-屏幕快照 2020-05-24 下午3.41.37.png)

    f是类别i的某一个样本的feature，c为类别i的类别中心feature，计算的时候用EMA在训练过程中动态改变:

    ![](Deep-Representation-Learning-on-Long-tailed-Data-A-Learnable-Embedding-Augmentation-Perspective-屏幕快照 2020-05-24 下午3.44.02.png)
2. **Featue Cloud**: 给定一个tail class的样本feature f, 用head class的angular distribution生成f周围的一群虚拟的feature cloud, head class的angular distribution的计算是不区分具体是head class中的哪一个而是直接把所有的head class取平均就好了:

    ![](Deep-Representation-Learning-on-Long-tailed-Data-A-Learnable-Embedding-Augmentation-Perspective-屏幕快照 2020-05-24 下午3.49.25.png)

**那么假设head class的angular distribution服从N(µ<sub>h</sub> and σ<sup>2</sup><sub>h</sub>), tail class的angular distribution服从N(µ<sub>t</sub> and σ<sup>2</sup><sub>t</sub>), 那么为了达到把head class的分布迁移过来，也就是把σ<sup>2</sup><sub>h</sub>迁移过来，在构建的feature cloud的时候需要保证，对于给定的一个tail class样本的feature x与随机从x的feature cloud中挑选的feature f，两者之间的角度α<sub>x</sub>需要服从N(0, σ<sup>2</sup><sub>h</sub> - σ<sup>2</sup><sub>t</sub>),这是两个独立同分布的性质.**

所以最后在训练的时候cosface和arcface的loss就可以更新为, 多出来一个α<sub>y</sub>就是一个**类似data augmentation的存在来增加样本以此增加diversity**:

![](Deep-Representation-Learning-on-Long-tailed-Data-A-Learnable-Embedding-Augmentation-Perspective-屏幕快照 2020-05-24 下午3.55.53.png)

那么关于head class/tail class的划分，论文中给了两种方式，一个就是通过卡样本数目T，如果某一个类别的样本数超过T就是head class否则就是tail class，另一种方式就是直接用样本数来加权得到整个数据集的variance, 那么很显然在计算过程中那些样本数比较多的类别会贡献更多的variance，所以再挨个算一遍每个类别的variance，如果单个类别的variance小于总体的variance说明就是tail class:

![](Deep-Representation-Learning-on-Long-tailed-Data-A-Learnable-Embedding-Augmentation-Perspective-屏幕快照 2020-05-24 下午3.58.46.png)

贴一个点, 能到sota，优势不大:

![](Deep-Representation-Learning-on-Long-tailed-Data-A-Learnable-Embedding-Augmentation-Perspective-屏幕快照 2020-05-24 下午4.00.55.png)

方法大体就是这个样子，感觉想法挺好的
