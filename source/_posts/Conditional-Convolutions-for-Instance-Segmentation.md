---
title: Conditional Convolutions for Instance Segmentation
date: 2020-03-13 14:39:58
tags:
  -- Segmentation
---
URL: https://arxiv.org/pdf/2003.05664.pdf

最新放出来的论文，做Instance Segmentation，和PolarMask一样都是follow FCOS大的框架，不同于Mask RCNN出框crop然后做Seg的方式，论文所提的方法更加类似FCN直接出，整个框架的设计感觉还是比较精巧的。具体看下怎么做：

![](Conditional-Convolutions-for-Instance-Segmentation-屏幕快照 2020-03-13 下午2.42.24.png)

我把这个结构分成两个部分:
1. FCOS: 也就是上图的上半部分，整个pipeline和FCOS没啥差别，只是head层的输出略有不一样，那么对于FPN每一个layer的每一个pixel，主要出三个东西:
  1. Classification Head: 和原版FCOS含义一样
  2. Center-ness Head： 这个定义也是和原版FCOS一致的，用来抑制不太好的预测结果
  3. Controller Head： 这个就是本篇论文的核心了，他负责出Mask FCN Head的参数，假设Mask FCN Head的总参数量是X，那么Controller Head的维度就是X，论文中当X取169的时候性能就比较不错了。
2. Mask FCN Head: Mask FCN Head是论文的核心点，上图下半部分，它的结构就是一般的FCN，但是它的特点在于FCN的参数是动态的，不同的instance有不同的参数，这就会造成多个Mask FCN Head的感觉，同时功能上也类似Mask RCNN出框的作用-区分Instance. Mask FCN Head接在P3 Layer之后, 经过几层Conv之后得到一个H x W x C的feature map, 论文中C = 8，作者claim C的取值对分割的性能影响不大. 甚至C = 2的时候性能也只是下降0.3%！因为Mask FCN Head负责出instance，而其参数又是由P3 - P7的head层所得，所以为了构建两者的联系，在Mask FCN Head输入层F<sub>mask</sub> Concat了F<sub>mask</sub>到P3 - P7的相对位移，假设F<sub>mask</sub>的维度为H x W x C，P<sub>i</sub>的维度为H<sub>i</sub> x W<sub>i</sub> x C<sub>i</sub>, 那么把F<sub>mask</sub>的每一个pixel映射到P<sub>i</sub>，映射前后坐标的offset就会和原始的F<sub>mask</sub> Concat到一起作为**Mask FCN Head**的输入。

最后模型的效果就是instance aware的segmentation:
![](Conditional-Convolutions-for-Instance-Segmentation-屏幕快照 2020-03-13 下午3.51.40.png)
