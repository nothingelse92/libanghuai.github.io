---
title: 'Cascade R-CNN: Delving into High Quality Object Detection'
date: 2018-09-24 18:16:03
tags:
  - Object Detection
  - Cascade
---
解决问题：
目前的检测模型通常需要通过设置IoU阈值来定义Positive和Negative bbox，作者发现最终检测模型的表现和IoU阈值的设置相关，通过下图的c，d可以发现低IoU阈值通常会对低IoU样本表现更好，因为不同IoU阈值下的样本分布不一样，一个IoU阈值很难对所有的样本都有效。因此为了解决这个问题，论文提出了Cascade R-CNN 模型。

![](Cascade-R-CNN-Delving-into-High-Quality-Object-Detection-image002.png)

论文实验的Cascade R-CNN模型基于典型的Two-Stage结构，Cascade R-CNN模型有多个Classification和Bounding Box header（下图中的Cx，Bx），前一个header refine过的Bounding Box 会作为下一个header的输入，并且IoU的阈值逐层增加，B0代表RPN的结果。通过这种方式实现对于每一级header输入数据的resample，保证了每一级header都有足够的正样本。

![](Cascade-R-CNN-Delving-into-High-Quality-Object-Detection-image003.png)

实验对比，可以发现cascade rcnn的效果还是有比较大的提升，但同时也大幅大增加了模型的复杂度

![](Cascade-R-CNN-Delving-into-High-Quality-Object-Detection-image004.png)

思考：
Cascade R-CNN结构是现有检测模型优化的思考方向，但是从上图第二章表中也可以看出加入cascade 结构在提高检测效果的同时也大幅度增加了模型复杂度.
