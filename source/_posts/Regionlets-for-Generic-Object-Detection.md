---
title: Regionlets for Generic Object Detection
date: 2019-05-21 13:44:36
tags:
  - Detection
---
URL:http://www.xiaoyumu.com/s/PDF/Regionlets.pdf
这是一个传统的用于物体检测的方法，论文的主要贡献是提出了regionlet的概念以及基于regionlet的物体检测方法论文定义物体检测中有三个范围概念：Bounding Box、Region、Regionlet，Bounding Box就是目标候选框，Region是用于Bounding Box的特征提取，位于Bounding Box内，作者认为Region的粒度过大不足以表示局部的特征，因此在Region内部提出更小的范围Regionlet：
![](Regionlets-for-Generic-Object-Detection-image002.png)

基于Regionlet的检测模型分为两个部分：
1. 提取每一个Regionlet的特征：这一步通常用HOG、LBP等特征来表示；
2. 融合每一个Regionlet的特征：对于Regionlet r，通过第一步我们可以得到他的特征T(r)，对于Region R利用下面的公式可以得到R具体的特征表示，算法从regionlet特征中选择一个一维特征（行或列），并从中选择特征最强的那个作为R的一个一维特征，具体的选择方式则通过boosting 模型来学习，文中采用RealBoost最后利用从regionlet中抽取到的特征来训练boosting分类器来选择最合适的bounding box。
![](Regionlets-for-Generic-Object-Detection-image003.png)
![](Regionlets-for-Generic-Object-Detection-image004.png)
具体的实验结果，方法提出的比较早所以实验结果可能并没有实际的参考价值：
![](Regionlets-for-Generic-Object-Detection-image005.png)
