---
title: Region Proposal by Guided Anchoring
date: 2019-03-18 21:59:46
tags:
  - Object Detection
---
URL:https://arxiv.org/abs/1901.03278
CVPR2019的一篇对anchor进行优化的论文，主要将原来需要预先定义的anchor改成直接end2end学习anchor位置和size。首先anchor的定义通常为(x, y, w, h) (x, y为中心点)，formulate一下：
![](Region-Proposal-by-Guided-Anchoring-屏幕快照 2019-03-18 下午10.33.58.png)
因此本文所提的guided anchoring利用两个branch分别预测anchor的位置和w、h：
![](Region-Proposal-by-Guided-Anchoring-屏幕快照 2019-03-18 下午10.28.29.png)

guided anchoring的主要内容有如下几点：
**Anchor Location Prediction**
逻辑很简单，利用一个1x1的conv将输入的feature map转换成 W x H x 1的heatmap，通过卡阈值t来得到anchor可能出现的位置，在训练的时候可以通过gt的框来生成heatmap的groudtruth，negtive、positive、ignore的pixel定义论文中有比较详细的介绍。
![](Region-Proposal-by-Guided-Anchoring-屏幕快照 2019-03-18 下午10.46.37.png)
**Anchor Shape Prediction**
这一部分逻辑和上一部分一样，也是通过一个1x1的conv将输入的feature map转换成W x H x 2的heatmap，只是考虑到如果直接回归w和h范围太广会比较不稳定，作者做了一定的转化将预测值约束到[-1,1],实际使用的时候再映射回去，s为feature map的stride，sigma为8：
![](Region-Proposal-by-Guided-Anchoring-屏幕快照 2019-03-18 下午10.41.23.png)
需要注意的是和传统的anchor设置不一样的是，guider anchoring在某一个pixel下只会设置一个anchor。
这一部分的训练其实会是比较需要特别注意的地方，论文中使用来IoU loss来监督，但是这样存在一个问题，因为这个分支本身是预测w，h的，所以IoU Loss的计算无法知道match的具体gt，作者提出的方法是sample 9组常见的w、h，这样就可以利用这9组w、h构建9个不同的anchor去和gt匹配，IoU最大的匹配gt就是当前需要去计算IoU Loss的gt，然后直接用heatmap的w、h和这个gt计算IoU Loss即可：
![](Region-Proposal-by-Guided-Anchoring-屏幕快照 2019-03-18 下午10.48.54.png)
**Anchor-Guided Feature Adaptation**
这一个模块主要是针对feature有可能和anchor不一致而提出的，因为对于原先预定义的anchor而言，每一个pixel对应位置的anchor其实都是一样的，所以也就无所谓feature的异同，但是guided anchoring逻辑下不同的pixel有可能anchor的size差别很大，仍然像之前那样直接出cls和reg很显然是不合适的，所以作者就提出了adaptation的模块，利用deformable conv来处理不同形状的anchor对应的feature。

论文的最后作者也提了一下因为GA-RPN可以得到很多高质量的porposal，通过提高阈值可以进一步优化检测的效果。
实验结果：
![](Region-Proposal-by-Guided-Anchoring-屏幕快照 2019-03-18 下午11.21.29.png)
