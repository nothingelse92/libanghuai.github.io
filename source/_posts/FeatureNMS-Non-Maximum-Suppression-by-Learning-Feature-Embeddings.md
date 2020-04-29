---
title: 'FeatureNMS: Non-Maximum Suppression by Learning Feature Embeddings'
date: 2020-04-18 17:37:06
tags:
  - Pedestrian Detection
  - NMS
---
URL: https://arxiv.org/pdf/2002.07662.pdf

从NMS的角度解决行人crowd的问题，解决方法很直接，抛开IoU从框面积的角度去解overlap问题，论文直接对每一个anchor再学习一个embedding feature去辅助判断不同的物体, 伪代码写的比较清楚,集合D就是最后的输出了:

![](FeatureNMS-Non-Maximum-Suppression-by-Learning-Feature-Embeddings-屏幕快照 2020-04-18 下午5.39.53.png)

做法上也很直接，在RetinaNet输出head，cls head + reg head之外再加一个embedding head，纬度为32维，当然这个值可以随意定了，监督方式Margin Loss:

![](FeatureNMS-Non-Maximum-Suppression-by-Learning-Feature-Embeddings-屏幕快照 2020-04-18 下午5.41.28.png)
![](FeatureNMS-Non-Maximum-Suppression-by-Learning-Feature-Embeddings-屏幕快照 2020-04-18 下午5.41.56.png)

f<sub>i</sub>和f<sub>j</sub>是两个不同的anchor对应的embeding feature，至于obj(i)就是用来判断anchor<sub>i</sub>属于哪一个object(gt box). NMS距离计算方面就是普通的L2.

β 和 α为两个超参数 : *α determines the margin between positive and negative examples, and the parameter β determines the decision threshold. (α = 0.2,β = 1.0)*

从结果上看当然是涨点了，但是感觉没有说服力，所有的对比没有和sota的方法进行一个公平对比，模型学习的设置也是和sota的方法完全不一样的，比如论文里应该只用了visible box来训练：

![](FeatureNMS-Non-Maximum-Suppression-by-Learning-Feature-Embeddings-屏幕快照 2020-04-18 下午5.48.08.png)
