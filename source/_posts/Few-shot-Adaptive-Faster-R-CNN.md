---
title: Few-shot Adaptive Faster R-CNN
date: 2020-02-25 12:33:24
tags:
  - Object Detection
  - Few Shot
  - Domain Adaptation
---
URL: http://openaccess.thecvf.com/content_CVPR_2019/papers/Wang_Few-Shot_Adaptive_Faster_R-CNN_CVPR_2019_paper.pdf

CVPR 2019的论文，这篇比较好的地方是在abstract部分就直接阐明了目前few shot问题存在的几个根本问题，我也比较认可论文提到的这几个痛点：
1. target domain数据量很少，从任务本身来说就比较难，从一个domain transfer到另一个domain，在如此受限的数据情况下
2. 同样也因为target domain数据量很少，导致过拟合问题是一个不得不处理的事情
3. few shot for detection目前来看实际工作不是很多，因为detection任务需要兼顾object定位和分类，所以任务本身上看比较难

我们具体看下这篇论文是怎么做的，整体PPL看上去还是比较复杂的:

![](Few-shot-Adaptive-Faster-R-CNN-屏幕快照 2020-02-28 下午2.41.16.png)

从大面上论文主要分成2个部分:
1. **Image Level Domain Adaptation**: 上图的上半部分就是Image Level Domain Adaptation部分，这里引入了一个SP操作（Split Pooling），这个主要是作者参考已有的论文结论觉得局部的patch可以更好的表示图片的特征。做法呢也比较简单，有一个初始框(w,h),随机再生成两个shift(δ<sub>w</sub>, δ<sub>h</sub>，这是相比较图像左上角的偏移)，这样就可以组成一个明确位置和大小的框了，然后因为是基于anchor的检测框架，所有这里的(w, h)是和anchor scale/anchor ratio保持一致的，论文里是给了9个框(3个ratio x 3个scale)，这样最后可以抽出9个patch的feature。那么论文的**Pair Sampling**则是GAN类似的对抗学习的过程，我们把这9个patch feature按照scale大小分成larege、medium、small三大部分，然后每一个部分分别来对抗学习，对于每一个部分组成两组pair:{s,s}, {s,t}(s代表source image,t代表target image)，然后GAN要学习的就是区分开这两者。
2. **Instance Level Domain Adaptation**: 上图的中间部分可以理解为正常的Faster RCNN逻辑，而下半部分就可以理解为这里的Instance Level Domain Adaptation，这一部分的作用倒也可以理解，adaptation的粒度不一样，这里作者提了一个概念叫*Instance ROI Sampling*. 相比较普通的ROI Sampling主要的差别就是IOU卡的很高(>0.7)这样得到的ROI更加贴近Install本身，另外就是所有的Positive都会送入到下一阶段做判断，而不是一般的RPN在处理的时候会控制前背景的比例，然后至于怎么去用GAN去区分就和*Image Level Domain Adaptation*里面提到的**Pair Sampling**逻辑是一摸一样的了。

那么针对作者提出的过拟合问题，论文了也给了一个解法:**Source Model Feature Regularization Training**. 想法比较直接，现在论文提出的FAFRCNN方法是针对src + target domain的，作者另外训练一个source domain的detector，然后给定相同的souce domain的数据让这两个模型提取出来的特征尽可能相近：

![](Few-shot-Adaptive-Faster-R-CNN-屏幕快照 2020-03-10 下午12.57.14.png)

然后论文的主要内容大概就这些，整体ppl有点太复杂不太实用。
