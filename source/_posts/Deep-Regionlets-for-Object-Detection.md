---
title: Deep Regionlets for Object Detection
date: 2019-05-25 13:48:28
tags:
  - Detection
---
URL:http://arxiv.org/abs/1712.02408
论文主要将regionlet的概念引入到CNN网络结构中提出了一个新的物体检测模型，检测模型主要分为两个部分：
1.  Region Selection Network：这一部分主要从RPN网络生成的bounding box中生成一些区域（Region），Region的生成借助STN网络（三层FC，6维输出）学习到的仿射变换矩阵来实现，因此这一部分输出的Region形状不一定是矩形，初始化时Region是整个boudding box的均分小矩形；
2.  Deep Regionlet Learning： 这一部分主要用来生成Regionlet以及对应的特征表示，Regionlet同样通过学习仿射变换矩阵从Region中生成，只是Regionlet最终的特征还借助于论文提出的Gating Network（多层FC） 来学习Rgionlet每一个位置的权重，最终两者的乘积会作为最后的输出特征。网络最后会接入Pooling层来融合特征。
![](Deep-Regionlets-for-Object-Detection-image002.png)
![](Deep-Regionlets-for-Object-Detection-image003.png)
在coco、voc上的实验结果：
![](Deep-Regionlets-for-Object-Detection-image004.png)
