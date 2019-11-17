---
title: Object Detection in Video with Spatiotemporal Sampling Networks
date: 2019-05-17 13:35:09
tags:
  - VID
---
URL:https://arxiv.org/pdf/1803.05549.pdf
这是ECCV2018 关于物体检测的一篇论文,论文主要提出一种时空采样网络STSN来提高视频中的物体检测效果。论文出发点还是整合多帧的信息来提高当前帧的检测效果，论文提出的STSN结构和FGFA比较类似，基本都会涉及到backbone网络提取特征、特征聚合等操作.

论文主要的贡献在于Spatiotemporal Feature Sampling，对于给定的帧I以及临近帧的范围K，I分别和K个临近帧形成pair作为STSN的输入，输入的两帧在经过Defornable CNN（基于ResNet-101）之后将输出concat到一起作为Spatiotemporal Feature Sampling 的输入，Spatiotemporal Feature Sampling部分同样利用Deformable CNN得到最后的offset field再结合临近帧的feature map经过一层deformable convolutional 得到最后的采样特征，所有pair采样之后得到的特征经过特征聚合作为detector的输入进行物体检测。STSN特征聚合部分和FGFA保持一致，大体流程论文中有比较简洁的示意。
![](Object-Detection-in-Video-with-Spatiotemporal-Sampling-Networks-image001.png)
量化结果：
![](Object-Detection-in-Video-with-Spatiotemporal-Sampling-Networks-image002.png)
效果图：
![](Object-Detection-in-Video-with-Spatiotemporal-Sampling-Networks-image003.png)
