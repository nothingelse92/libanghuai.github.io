---
title: 'CityPersons: A Diverse Dataset for Pedestrian Detection'
date: 2020-01-21 14:33:47
tags:
  - Pedestrian Detection
---
URL:https://arxiv.org/abs/1702.05693

做行人检测比较经典的数据集了，CityPersons，数据集是基于cityscapes的数据集进行refine的，CityPersons选择了CityScapes中精标注的5000张图片进行标注的(来自欧洲27个城市)，所谓的精标注就可以理解为标准的instance segmentation的标注，共有30类的类别标签，per pixel标注。CityPersons只标注CityScapes中Person和Rider两类，并将两类进一步细分为：**pedestrian**(walking, running or standing up), **rider**(riding bi- cycles or motorbikes),**sitting person**,and **other person**(with unusual postures, e.g. stretching)。

标注方法可以参考下图，对于Pedestrian和Rider**可见框就是seg标注的最小外接矩形**,**全身框的话则是先标头顶到两脚中间点的直线，然后按照固定的长宽ratio 0.41拓展出框的宽度从而完成整个全身框的标注**，至于遮挡的比例那就是上述两个面积的比值，而至于**其他两个类别则直接用的seg的最小外接矩形没有再标注全身框**，而对于假人这样的object直接标为ignore：

![](CityPersons-A-Diverse-Dataset-for-Pedestrian-Detection-屏幕快照 2020-01-21 下午3.24.25.png)

数据集划分：

![](CityPersons-A-Diverse-Dataset-for-Pedestrian-Detection-屏幕快照 2020-01-21 下午3.43.44.png)

那么在后续的论文中其实我们还会比较常见Heavy，Bare，Partial这样的划分，这个划分是Repulsion Loss那篇论文中根据CityPersons的Reseanable子集继续细分出来的，具体的可以参考Repulsion Loss这篇论文，主要是根据遮挡程度来划分的。
