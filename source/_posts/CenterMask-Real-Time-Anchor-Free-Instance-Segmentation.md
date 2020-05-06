---
title: 'CenterMask : Real-Time Anchor-Free Instance Segmentation'
date: 2020-05-06 12:44:11
tags:
  - Segmentation
---
URL: https://arxiv.org/abs/1911.06667

CVPR2020的一篇分割的论文，整体novelty有限，相当于将FCOS应用到分割领域中:

![](CenterMask-Real-Time-Anchor-Free-Instance-Segmentation-屏幕快照 2020-05-06 下午12.45.45.png)

上图就是论文所提的方法了，前半截就是标准的FCOS的流程，一个Regression分支+一个Classification分支+一个Centerness分支，后半截就是新加入的Mask分支，做法和Mask RCNN做法是一致的，首先利用Regression分支出的框去Crop Feature，这里面用的还是ROIAlign只是具体去到哪个FPN Layer上去Crop Feature作者在论文中做了一些说明，但实际上和Mask RCNN还是没差到哪去，只是在FCOS的框架下做了一下适配。

Mask分支的做法是这样的，首先Crop出来的14x14的Feature**经过4层conv**送入到一个叫SAM的Attention模块中，SAM模块呢做了这么几个事：

1. 分别经过两个Pooling，一个是Average Pooling ，一个是Max Pooling，然后把这两个Pooling的输出Concat到一起得到F<sub>concat</sub>，至于这样做有啥Insight就不太清楚了。
2. F<sub>concat</sub>再经过一个Conv得到新的Feature F<sub>new</sub>。
3. F<sub>new</sub>通过Sigmoid之后和原始的Feature相乘得到Attention之后的Feature F<sub>attention</sub>。
4. F<sub>attention</sub> 2倍Up-Sample之后再1x1去分类得到最终的结果.

作者另外的改进就是VoVNetV2 backbone的改进了，两处:
1. VoVNet + residual connection
2. SENet -> eSENet（SENet原来是2层FC，第一层FC会砍channel数，第二层FC会还原回去，eSENet就省掉了第一个FC直接上第二个FC增加了不少的计算量，原因是作者认为第一个FC降channel会丢失channel的信息）


各个模块的ablation(mask scoring就是Mask Score RCNN里面的方法):

![](CenterMask-Real-Time-Anchor-Free-Instance-Segmentation-屏幕快照 2020-05-06 下午12.58.31.png)
