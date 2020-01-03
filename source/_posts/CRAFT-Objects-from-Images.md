---
title: CRAFT Objects from Images
date: 2018-12-14 23:36:43
tags:
  - Object Detection
---
URL:https://arxiv.org/abs/1604.03239
CVPR2016的一篇论文。首先CRAFT代表 Cascade Region proposal network And FasT-rcnn，本论文主要想解决RPN网络生成的区域不太精确的问题，比如对于一些外观复杂度较低的事物如树木，会因为RPN网络产生的背景区域的存在导致比较难检测或者产生FP，因此作者尝试利用cascade的方式来解决这样的问题。
CRAFT模型可以分成两个部分：
1. Cascade Proposal Generation：这一部分同样可以分成两个部分，RPN和FRCN，FRCN其实就是一个Fast R-CNN网络，两个网络分别训练，RPN网络生成相对粗糙的区域，FRCN用RPN的输出作为输入对RPN的结果进行进一步的refine，提高了候选区域的质量也减少了背景框：
![](CRAFT-Objects-from-Images-image002.png)
2. Cascade Object Classification：Cascade Object Classification部分由两个FRCN网络级联而成，Cascade Proposal Generation部分的输出会作为FRCN1的输入，FRCN1的输出摒弃掉“背景”类作为FRCN2的输入，实现细节上两个模型复用参数。这里需要注意的是，作者认为为了学习类间的细微差别，模型复用了被Fast RCNN抛弃的 One-vs-Rest的分类方式，用N个二分类的交叉熵损失函数的和作为最终的loss。
![](CRAFT-Objects-from-Images-image003.png)
最终在VOC上面的表现效果：
![](CRAFT-Objects-from-Images-image004.png)
