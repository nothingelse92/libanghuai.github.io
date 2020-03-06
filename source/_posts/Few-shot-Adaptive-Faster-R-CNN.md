---
title: Few-shot Adaptive Faster R-CNN
date: 2020-02-25 12:33:24
tags:
  - Object Detection
  - Few Shot
---
URL: http://openaccess.thecvf.com/content_CVPR_2019/papers/Wang_Few-Shot_Adaptive_Faster_R-CNN_CVPR_2019_paper.pdf

CVPR 2019的论文，

这篇比较好的地方是在abstract部分就直接阐明了目前few shot问题存在的几个根本问题，我也比较认可论文提到的这几个痛点：
1. target domain数据量很少，从任务本身来说就比较难，从一个domain transfer到另一个domain，在如此受限的数据情况下
2. 同样也因为target domain数据量很少，导致过拟合问题是一个不得不处理的事情
3. few shot for detection目前来看实际工作不是很多，因为detection任务需要兼顾object定位和分类，所以任务本身上看比较难

我们具体看下这篇论文是怎么做的，整体PPL看上去还是比较复杂的:

![](Few-shot-Adaptive-Faster-R-CNN-屏幕快照 2020-02-28 下午2.41.16.png)
