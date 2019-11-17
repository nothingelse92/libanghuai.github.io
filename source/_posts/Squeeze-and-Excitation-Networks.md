---
title: Squeeze-and-Excitation Networks
date: 2018-11-15 01:03:20
tags:
  - Detection
---
URL:https://arxiv.org/abs/1709.01507 Github:https://github.com/hujie-frank/SENet
CVPR2017的一篇论文，CNN网络卷积操作本身是整合空间信息和channel信息的，论文作者的motivation是显式地对channel做attention来提高模型的表达能力，论文主要的内容就是一个SE Block：
![](Squeeze-and-Excitation-Networks-image002.png)
SE Block主要分成两个部分：
1.      Squeeze：上图中的Fsq, 本质就是一个Global Average Pooling，将H x W x C映射成1x1xC：
![](Squeeze-and-Excitation-Networks-image003.png)
2.      Excitation：上图中的Fex, 本质就是两个全连接层来学习channel的权重信息，W1是第一层FC参数，输出为C/r，W2为第二层FC参数，输出恢复到C，r为压缩比例，用来减少参数量：
![](Squeeze-and-Excitation-Networks-image004.png)
最后将excitation得到的输出施加到对应的channel上就可以得到最终的输出（图中的Fscale）
SE Block本身的设计是一个嵌入模块，所以它可以方便的结合现有的网络结构，论文中给了两个示例SE-Inception和SE-ResNet：
![](Squeeze-and-Excitation-Networks-image005.png)
![](Squeeze-and-Excitation-Networks-image006.png)
论文中还重点论述了SE Block不会对原模型带来比较大的复杂度，对于输入图片大小为224x224时，ResNet50约3.86GFLOPS，SE-ResNet-50约3.87GFLOPS，实际运行时间两者差别也不是很大，贴一张具体的数据：
![](Squeeze-and-Excitation-Networks-image007.png)
