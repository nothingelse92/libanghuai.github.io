---
title: Meta-Learning to Detect Rare Objects
date: 2020-04-24 12:47:27
tags:
  - Object Detection
  - Few Shot
---
URL: http://openaccess.thecvf.com/content_ICCV_2019/papers/Wang_Meta-Learning_to_Detect_Rare_Objects_ICCV_2019_paper.pdf

ICCV 2019的论文，做few shot detection. 论文的insight作者说的比较清楚 *"we introduce a parameterized weight prediction meta-model that is trained on the space of model parameters to predict a category’s large-sample bounding box detection parameters from its few-shot parameters. The"*, 相同类别的一些数据当数据量比较小的时候(few shot)以及数据量比较大的时候学习到的分类信息肯定是不一样的，那么作者的出发点呢就是bridge这两者的差异，学习一个**weight prediction meta-model**来实现从few shot参数到large shot的参数转化。

![](Meta-Learning-to-Detect-Rare-Objects-屏幕快照 2020-04-27 下午12.29.27.png)

作者将CNN模型里面的所有参数区分为类别有关的和类别无关的两种，比如对于Faster RCNN，backbone和RPN是类别无关的，而Fast RCNN部分是类别相关的。对于类别无关的参数, 直接在fine tune的时候用来初始化就好了，因为特征是类间共享的。而对于类别有关的参数，就学习一个**weight prediction meta-model T**来实现few shot参数到large shot参数的的转化。

![](Meta-Learning-to-Detect-Rare-Objects-屏幕快照 2020-04-27 下午12.35.11.png)

上图呢就是在一个episode里面学习的loss监督，假设对于类别c，w<sub>det</sub><sup>c,\*</sup>代表基于large shot训练出来的参数(fast rcnn的分类+定位参数), w<sub>det</sub><sup>c</sup>代表基于few shot训练出来的对应的参数，那么作者参考以前的论文将w<sub>det</sub><sup>c</sup>到w<sub>det</sub><sup>c,\*</sup>的转化定义为一个回归任务φ,φ就是用一个FC实现的，所以loss上的前半截就是直接约束这个回归任务，loss的后半截就是一个标准的cls+reg的loss。

那么最终在训练的时候呢，第一步在全量数据上先训练一个基本的检测器，第二步呢fix住模型无关的参数(backbone + rpn), 放开模型相关的参数在MAML配置下联合训练w<sub>det</sub><sup>c,\*</sup>和w<sub>det</sub><sup>c</sup>，有点蒸馏的感觉。
在Meta-testing阶段，meta training阶段学到的weight prediction meta-model T参数不变作为一个regulization项来监督novel class的fine tune.

实验结果, 放到现在点也不算高了，那篇直接fine tune head的论文可以把点刷到30+了:

![](Meta-Learning-to-Detect-Rare-Objects-屏幕快照 2020-04-27 下午12.45.09.png)
