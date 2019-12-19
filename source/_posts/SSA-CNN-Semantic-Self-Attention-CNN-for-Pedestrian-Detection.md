---
title: 'SSA-CNN: Semantic Self-Attention CNN for Pedestrian Detection'
date: 2019-12-19 13:21:07
tags:
  - Pedestrian Detection
  - Segmentation
  - Attention
---
URL: https://arxiv.org/pdf/1902.09080.pdf

关于行人检测的论文，出发点的话感觉和这篇论文是比较类似的[Mask-Guided Attention Network for Occluded Pedestrian Detection](http://libanghuai.com/2019/12/08/Mask-Guided-Attention-Network-for-Occluded-Pedestrian-Detection/)。主要想利用mask作为额外的监督来辅助提升检测的效果。两篇论文的Segmentation的标注也都是粗粒度的，MGAN是利用可见框标注作为mask，本篇论文是直接用的全身框作为mask。至于具体的做法可以参考论文里给的这张图：

![](SSA-CNN-Semantic-Self-Attention-CNN-for-Pedestrian-Detection-屏幕快照 2019-12-19 下午10.04.51.png)

整体框架结构依然follow的Faster RCNN逻辑，论文所提的SSA-CNN主要分成两个部分：**SSA-RPN**和**SSA-RCNN**:
1. SSA-RPN部分，从backbone（VGG16）的conv4_3和conv5_3分别引出一个segmentation分支，这个seg分支本身用全身框的mask去监督，这个分支的feature map然后再和对应的conv4_3或者conv5_3 feature map concat到一起引出正常的RPN业务分支cls和bbox，然后用预定义的target anchor去监督。有一个需要注意的地方是 **只有conv5_3这个分支出的预测结果才会被接下来的SSA-RCNN使用，conv4_3这个分支出的seg类似一个外挂只做额外的监督，inference的时候不用**
2. SSA-RCNN部分，这个部分和传统的Faster RCNN的Fast RCNN分支处理不完全一样，一般的Fast RCNN是利用RPN的proposal经过ROIPooling得到对应的feature去做cls和reg，但对于SSA-RCNN则是直接利用SSA-RPN的proposal去原图中抠取，然后padding resize之后作为SSA-RCNN的输入，这样做的原因作者也在论文中阐明了: **the pooling bins collapse if ROI’s input resolution is smaller than output**，比如输入112x112映射到ROIPooling那就对应着7x7，但是CityPersons和 Caltech数据集有大量小于112 x 112大小的人，所以这个问题就会变得比较严重。其他SSA-RCNN模型部分就和SSA-RPN很像了，conv4_3和conv5_3引出的seg分支的feature map会先concat到一起，然后再和conv5_3 concat然后接入cls+reg来做行人的定位。

这篇论文其他应该就没有需要注意的了。
