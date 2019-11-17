---
title: 'PFLD: A Practical Facial Landmark Detector'
date: 2019-03-02 10:20:45
tags:
  - Landmark
---
URL: https://arxiv.org/abs/1902.10859
相关主页：https://sites.google.com/view/xjguo/fld
这两天刚挂出来的关于landmark的论文，论文中对300w数据集报的点是要比LAB、SAN等CVPR2018论文的点是要高的，在845手机上也可以达到140FPS的速度，论文所提方法主要在设计加权的loss，比如考虑人脸的yaw、pitch、roll等信息来加权loss。

论文首先总结了一下目前landmark检测的一些问题，比如局部遮挡、局部光照、人脸Pose、数据不均衡等，同时计算平台对算力的限制也是一个需要关注的点。除了最后的减少模型size是通过尝试不同的backbone网络，其他的论文所提问题可以理解为都集中在Loss的设计：
![](PFLD-A-Practical-Facial-Landmark-Detector-屏幕快照 2019-03-02 上午10.22.31.png)
而整个Loss的设计又可以理解为加权的权重该以什么逻辑加上去，上式是论文中所提的Loss：
m是人脸数，n为landmark数，Wnc是当前脸所属的类别c所占总脸数的比例的倒数，theta为具体的角度，k为yaw、roll、pitch，dnm为距离计算的方式比如L1，L2.至此Loss的具体含义就不用赘述了；
![](PFLD-A-Practical-Facial-Landmark-Detector-屏幕快照 2019-03-02 上午10.22.27.png)
这个是论文中所提出的整个网络结构：
backbone基于mobilenet v2，为了得到人脸的yaw、pitch、roll等值（默认送进网络的图经过align所以不需要考虑其他的状态信息），作者在backbone的基础上又加了一个分支来专门出这几个值，至于这几个值gt的生成是通过mean face直接计算得到的。

在300w数据集上的表现（ION和IPN是不同的距离norm方式）：
![](PFLD-A-Practical-Facial-Landmark-Detector-屏幕快照 2019-03-02 上午10.22.48.png)
