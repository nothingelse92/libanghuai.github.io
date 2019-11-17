---
title: Deep Mutual Learning
date: 2018-12-15 20:47:39
tags:
  - Mutual learning
  - Classification
---
URL: http://cn.arxiv.org/pdf/1706.00384v1
论文主要在讲mutual learning相互学习，deep mutual learning整体感觉和model distillation还是比较像的,只是不是用训练好的大网络来带小网络而是用一些网络相互同步学习来提高模型整体的效果:
![](Deep-Mutual-Learning-屏幕快照 2018-12-15 下午8.49.05.png)

对于其中的单个网络除了本身分类的cross entropy loss外还会涉及到KL散度来度量两个网络预测结果之间的loss（K是网络个数）：
![](Deep-Mutual-Learning-屏幕快照 2018-12-15 下午8.49.47.png)
![](Deep-Mutual-Learning-屏幕快照 2018-12-15 下午8.50.11.png)
![](Deep-Mutual-Learning-屏幕快照 2018-12-15 下午8.50.24.png)
补充一些作者针对mutual learning的有效性做的一些探究：
+ 作者认为DML相对独立的模型得到了wider minima，因此更加鲁棒，作者设计了一组实验对网络的参数增加了一些噪声，下图是增加噪声前后的结果，正常比较的时候两者相似，增加噪声扰动之后DML效果相对更好：
![](Deep-Mutual-Learning-屏幕快照 2018-12-15 下午8.52.25.png)
+ K个模型的mutual learning通常可以有两种表现形式，一个就是当前这个模型和其余的K-1个模型分别进行KL distance计算，另一个就是当前模型只要和其余K-1个模型的预测结果均值来进行KL distance计算，作者实验发现后者其实效果会更差，作者认为是多模型结果的融合影响了一些网络细节的teaching signal:
![](Deep-Mutual-Learning-屏幕快照 2018-12-15 下午8.51.58.png)
