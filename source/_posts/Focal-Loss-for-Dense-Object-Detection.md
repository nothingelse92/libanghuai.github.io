---
title: Focal Loss for Dense Object Detection
date: 2020-01-03 19:55:52
tags:
  - Loss
  - Classic
  - Object Detection
---
URL: http://openaccess.thecvf.com/content_ICCV_2017/papers/Lin_Focal_Loss_for_ICCV_2017_paper.pdf

重读经典系列第三篇：RetinaNet

ICCV 2017的Best Student Paper,也是 He Kaiming一篇很有代表性的工作，论文主要focus在One Stage Detector中样本不均衡这件事上，并且提出了**Focal Loss**来解决这样的问题，同时基于Focal Loss实现了一个One Stage Detector **RetinaNet**,可以达到Two Stage Detector的精度同时可以保持One Stage Detector的速度。

我们知道Two Stage Detector精度高速度慢，One Stage Detector速度快精度低，这几乎是所有人可以脱口而出的特性，那么作者认为One Stage Detector精度低的主要原因就是非常严重的class imbalance问题，基于anchor的检测器动则有10W+的anchor数目，其中Positive的anchor只有几十个，这样正负样本比几乎可以达到1:1000，大量的负样本中有很多的easy negative samples它们对模型的训练几乎无法贡献有效的信息,同时大量的这种样本本身对模型的训练也是很有害的，毕竟它们占据主要部分容易主导模型的训练。因此作者提出了Focal Loss：
**FL(p<sub>t</sub>) = −α<sub>t</sub>(1 − p<sub>t</sub>)<sup>γ</sup> log(p<sub>t</sub>)**
那么Focal Loss本身呢是来自于Cross Entropy Loss(以二分类为例):
![](Focal-Loss-for-Dense-Object-Detection-屏幕快照 2020-01-03 下午8.14.46.png)
稍微简化一下：
![](Focal-Loss-for-Dense-Object-Detection-屏幕快照 2020-01-03 下午8.15.32.png)
那么CE(p, y) = CE(p<sub>t</sub>) = − log(p<sub>t</sub>)
那么我们再来看看Focal Loss在CE Loss基础上增加的东西：
1. −α<sub>t</sub>: 这就是简单的一个类别权重，比如可以将正样本的权重加大也是缓解class imbalance的一个选择
2. (1 − p<sub>t</sub>)<sup>γ</sup> : 这个可以理解为Focal Loss的核心吧，会整体通过模型的预测值动态的去调整loss的权重，如果某一个sample模型预测的类别是错误的那就意味着p<sub>t</sub>值会比较小（注意看p<sub>t</sub>的定义，对于每一个类别都是如此），那么Focal Loss整体就会和CE Loss差不多不会有什么影响，如果一个easy sample可以被模型很好的分类那么意味着p<sub>t</sub>值会比较大，那么Loss的权重就会变小从而优化过程中不会刻意处理，因此整个模型训练过程中都会刻意去优化hard sample。其中γ是平滑系数，论文中通过尝试γ = 2效果会比较好.

至于论文中提到的RetinaNet整体其实没有什么特殊的，具体结构如下，是一个FPN的结构：
![](Focal-Loss-for-Dense-Object-Detection-屏幕快照 2020-01-03 下午8.42.36.png)
需要注意的是:
1. class subnet 和 box subnet参数在P3 - P7之间是共享的
2. P3 - P5通过Conv来源于C3 - C5，P6通过Conv来源于P5，P7通过Conv来源于P6
3. P3 - P7的结果会concat到一起最后一起NMS
4. 为了训练的稳定性初始化有一些trick具体可以参考原论文
