---
title: >-
  Learning Efficient Single-stage Pedestrian Detectors by Asymptotic
  Localization Fitting
date: 2019-12-10 00:23:20
tags:
  - Pedestrian Detection
---
URL: http://openaccess.thecvf.com/content_ECCV_2018/papers/Wei_Liu_Learning_Efficient_Single-stage_ECCV_2018_paper.pdf

ECCV2018行人检测的论文，在清一色的Faster RCNN系列文章中也算是一股清流了，论文用Single Shot的模型来做行人检测，也是为数不多把模型速度作为卖点和核心的论文。论文的整个核心可以理解为Cascade RCNN + RefineDet(ALF思想类似Cascade RCNN、One Stage的解决方案类似RefineDet，毕竟Cascade RCNN和RefineDet本身有些设计理念是很像的).


![](Learning-Efficient-Single-stage-Pedestrian-Detectors-by-Asymptotic-Localization-Fitting-截屏2019-12-1123.53.08.png)

论文的核心概念是Asymptotic Localization Fitting(ALFNet), 上图示意图还是比较明显的，3个橙色feature maps是resnet/mobilenet的stage 3、4、5，绿色是直接在stage 5上接conv延伸出来的一层feature map，整体构成了类似FPN的逻辑。
在每个stage上都会通过CPB模块渐进式对框进行refine，这一步就和Cascade RCNN的逻辑比较像了，每个CPB都会对anchor进行一次refine，每次正负样本的IOU阈值都逐步提高，从而不断的提高框的回归精度，这里面有一个需要注意的地方就是anchor的生成只用了一个ratio: 0.41...就是CityPerson数据集的统计值，这还是有点hack数据集的意思的...
