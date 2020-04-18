---
title: Prototypical networks for few-shot learning
date: 2020-04-18 16:48:51
tags:
  - Classification
  - Few Shot
---
URL: https://arxiv.org/pdf/1703.05175.pdf

NIPS的一篇论文，few shot learning比较早期的工作了，这个论文的做法和simple shot是很类似的，当然simple shot在后了，首先作者定义一个embedding function f可以将经过feature extractor的图片映射到高维空间,在这个高维空间上找到各个class的类别中心(prototype)，通过距离计算将query图片assign到对应的类别中心完成分类：

c<sub>k</usb> 就是类别k的prototype, S<sub>k</sub>为类别属于k的样本集合，f<sub>φ</sub>就是上面说的那个embedding function，x<sub>i</sub>就是输入图片经过feature extractor得到的特征了.

![](Prototypical-networks-for-few-shot-learning-屏幕快照 2020-04-18 下午4.52.53.png)

类别归属的判断, d就是一种距离的度量，论文里面就直接用的欧式距离:

![](Prototypical-networks-for-few-shot-learning-屏幕快照 2020-04-18 下午4.56.27.png)

论文提供的伪代码还是比较清楚的:

![](Prototypical-networks-for-few-shot-learning-屏幕快照 2020-04-18 下午4.58.37.png)

感觉整个论文的核心就在这了，因为是投的nips论文另外的笔墨都花在了机制的解释和论证上了，有兴趣可以直接看原文
