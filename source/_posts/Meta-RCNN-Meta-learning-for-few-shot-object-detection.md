---
title: 'Meta-RCNN: Meta learning for few-shot object detection'
date: 2020-05-13 11:51:53
tags:
  - Few Shot
  - Object Detection
---
URL：https://openreview.net/pdf?id=B1xmOgrFPS

ICLR2020一篇866被据掉的论文，和ICCV的另一篇Meta RCNN重名了，整个思路相似度也挺高的，出发点是在每个meta task训练的过程中让模型刻意只注意当前meta task涉及到的C类，而忽略其他的类别，所以模型在训练过程中会生成一个attention(class级别)，然后作用于原始的图片feature作为后续的处理输入.和ICCV那篇Meta RCNN不同的地方是那篇attention是作用于ROI Pooling之后的feature。 
