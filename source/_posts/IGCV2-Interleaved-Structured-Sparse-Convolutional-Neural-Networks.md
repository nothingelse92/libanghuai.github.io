---
title: 'IGCV2: Interleaved Structured Sparse Convolutional Neural Networks'
date: 2019-01-20 21:17:25
tags:
  - Detection
---
URL: https://arxiv.org/pdf/1804.06202
IGCV2, CVPR2018, 主要把IGCV1中提到的group convolution进行了推广，将卷积操作变的更加稀疏，以此来达到减少冗余的目的。
在IGCV1论文中作者把IGC操作抽象成如下的表达式，x为输入，x前面的表达式整体是一个dense convolution kernel：
![](IGCV2-Interleaved-Structured-Sparse-Convolutional-Neural-Networks-f1ed32b809c7dda6f6c1b5a8834e594e6c0b99ce.png)

本论文中提到的Interleaved Structured Sparse Convolution就是它的一般表达：
![](IGCV2-Interleaved-Structured-Sparse-Convolutional-Neural-Networks-c181db945a7431adc7c182e16398ada2cc287be4.png)
那么为了保证输入x前面的表达式同样是dense convolutional kernel，论文中做了条件约束：前面一级group convolution里不同partition的channel在后面一级的group convolution要在同一个partition里面, 那么follow IGCV1的结构设计第一级group convolution为spatial conv (3x3)其余为1x1:
下图是wangjingdong的PPT上对IGCV2结构的描述示意图，比论文里面的示意图要清晰直观很多，其实就是把IGCV1的第二个group convolution再划分成多个1x1的group convolution操作 ：
![](IGCV2-Interleaved-Structured-Sparse-Convolutional-Neural-Networks-6268924f405c5224270e8fe5c80de272c04dbfa2_1_690x286.png)
实验Group Convolution的组数对结果的影响：
![](IGCV2-Interleaved-Structured-Sparse-Convolutional-Neural-Networks-b493611cc06c1996f7187b42c8cceaffa7452a20_1_539x500.png)
和其他一些网络模型的结果比较：
![](IGCV2-Interleaved-Structured-Sparse-Convolutional-Neural-Networks-e6c4c428b992b470ad7c2655ef49528184b321e9_1_608x500.png)
