---
title: Diversity Transfer Network for Few-Shot Learning
date: 2020-05-12 23:59:24
tags:
  - Few Shot
  - Classification
---
URL: https://arxiv.org/pdf/1912.13182.pdf

AAAI 2020的一篇做Few shot的论文，论文的思想可以理解为对feature做augmentation来增强模型的多样性，希望在novel class也达到比较好的效果.

![](Diversity-Transfer-Network-for-Few-Shot-Learning-394d9577106a1e1ebc5b29013c25f2a0f415a56c.png)

上面这张图就是论文所提的方法了，主要两个部分：

1. 橙色的Meta Task: 输入有三种图，Query Image和 Support Image就是标注的episode设置，在此基础上额外加了一组Reference Image，它由H组类别相同的图片对组成。那么首先这些所有的图都会过一个Feature Extractor进行特征提取(特征会经过Norm)，然后一组Reference Image 图片对的输出feature会相减和Support Image的feature再相加送入到一个Generator里面进行encoding，那么作者认为这个Encoding之后的feature和原始的support image的feature表征的是同一类物体(毕竟相同类别的两张图相减了嘛)，作者通过这样的操作把intra-class的diversity显式的encode到网络的训练过程中，希望模型可以学习到这种多样性，至于meta class的分类就和protonet一样了, 只是proxy的计算是H个encoding的feature再加上原始的support image的feature取平均得到：

  ![](Diversity-Transfer-Network-for-Few-Shot-Learning-d5f6b0f3151d960602c16b9bbb15e45b4a56e689.png)

  2. 灰色的分支就是一个普通的分类任务，目的是加速模型的收敛，作者在基础上作了一些优化。

贴一下miniImageNet的点, 相比现在的SOTA就不是很高了:

![](Diversity-Transfer-Network-for-Few-Shot-Learning-f659232dcbd0bec01b5f4feacd93cbba6178b503.png)
