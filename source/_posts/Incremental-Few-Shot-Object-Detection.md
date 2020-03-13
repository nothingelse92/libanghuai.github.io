---
title: Incremental Few-Shot Object Detection
date: 2020-03-13 12:00:53
tags:
  - Object Detection
  - Incremental Learning
  - Few Shot
---
URL: https://arxiv.org/abs/2003.04668

CVPR2020的论文，论文算是提出了一个新的子任务，Few Shot + Incremental Learning，这个任务论文中也给了明确的定义：
1. 首先模型的训练是可以基于量比较大的base class的数据来训练的，这样可以得到一个还不错的检测模型
2. novel class的数据可以随时*注册*，同时数据量很少，这样模型可以做到随时部署同时可以cover新增的类别

![](Incremental-Few-Shot-Object-Detection-屏幕快照 2020-03-13 下午12.10.03.png)

然后我们具体来看看论文是怎么做的, 首先模型方面被拆成两个部分
1. feature extractor: 论文中描述为class-agnostic, 相当于这一块是类别无关的base class和novel class公用这一部分用来特征提取
2. class code generator: 顾名思义，学习不同类别的编码，和上述提取出来的feature整合作为最后detection的输入

模型的学习也按照上面的两个模块分成了两个stage：

stage I: **feature extractor**的学习，这部分内容没有特别的地方就是CenterNet，训练完之后把detection head去掉之后就是feature extractor了

stage II: **class code generator**的学习, 这一部分的理解需要把CenterNet heatmap输出那一层进行一定的分解，假设feature extractor最后输出的feature map维度是H x W x C，总共有N个类别，那么逻辑上来讲heatmap维度应该是h x w x N，这个维度转化可以通过1 x 1的卷积达到目的，那么分解一下对于其中的一层feature map相当于H x W x C经过 1 x 1 x C的conv得到！

作者把这个理解为class independent的一个编码，这也是feature map做法的好处类别之间是相对独立的. *class code generator* 就被定义成学习这个class independent的编码. 至于怎么学呢, meta learning, 每一轮学习通过sample support集和query集去refine模型，对于给定一组输入，走 *feature extractor* 得到对应的feature，走 *class code generator* 得到对应的code，两者一结合就可以得到一个h x w的heatmap，然后拿这个heatmap去和gt做L1 loss从而来监督*class code generator*的学习

那么在Inference的时候呢对于base class的数据就直接上训练好的CenterNet，对于novel class的数据就分别走一遍*feature extractor*和*class code generator*然后得到最后的输出，这就是最后的定位结果。
