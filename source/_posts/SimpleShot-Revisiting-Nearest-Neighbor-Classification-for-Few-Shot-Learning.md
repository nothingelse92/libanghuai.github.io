---
title: 'SimpleShot: Revisiting Nearest-Neighbor Classification for Few-Shot Learning'
date: 2020-04-10 11:58:39
tags:
  - Few Shot
  - Classification
---
URL: https://arxiv.org/pdf/1911.04623.pdf

一篇打脸的论文，现在做few shot任务的方法通常有meta learning/metric learning等方法，作者在论文里claim其实只要对feature extrator输出的feature进行最近邻统计就可以在few shot benchmark上得到很高的指标，这一类简单且高效的few shot方法可以参考另外两篇论文：*Rethinking Few-Shot Image Classification: a Good Embedding Is All You Need?*, 以及做检测的: *Frustratingly Simple Few-Shot Object Detection*。 论文很简短，做法很简单，效果很不错。

看看论文怎么做的吧，首先数据集上面会划分成D<sup>base</sup>, D<sup>noval</sup>, 然后用一般的CNN网络基于D<sup>base</sup>去训练一个分类器，这里作者也用了五个不同的backbone，Conv-4，WRN-28-10， DenseNet-121，ResNet-10/18，MobileNet.

当分类模型训练好以后变成feature extractor，对novel class里面的test数据提取特征，同时也对novel class里的support数据提取特征，通过计算两者的相似度(最短距离)来match test数据的label，如果support数据集里每一类的图片有多张K-shot，那么就做个平均作为这一类的类别中心，test图片的feature就和这个类别中心算最短距离就好
。

然后对于feature extractor的数据作者也做了一些ablation，一是比做任何操作，直接去计算距离，二是L2 norm一下，三是对feature减均值再做norm，作者分别做了比较详细的对比实验：


![](SimpleShot-Revisiting-Nearest-Neighbor-Classification-for-Few-Shot-Learning-屏幕快照 2020-04-10 下午12.44.47.png)
