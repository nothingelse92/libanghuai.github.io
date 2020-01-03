---
title: Bi-box Regression for Pedestrian Detection and Occlusion Estimation
date: 2019-12-22 14:34:16
tags:
  - Pedestrian Detection
---
URL: http://openaccess.thecvf.com/content_ECCV_2018/papers/CHUNLUAN_ZHOU_Bi-box_Regression_for_ECCV_2018_paper.pdf

ECCV2018的一篇行人检测的问题，论文的做法其实比较简单，就是让模型在学习全身框的同时也出可见框的结果，两者可以起到互补的作用，整体模型结构也比较简单就是在backbone之后接两个平行的业务层，一个出可见框的结果一个出全身框的结构，两个平行业务层的结构是一致的，具体的逻辑可以参考论文里的下面这张图：

![](Bi-box-Regression-for-Pedestrian-Detection-and-Occlusion-Estimation-屏幕快照 2019-12-22 下午2.40.42.png)

那么这篇论文需要注意的更多的是一些细节：
  1. 虽然模型最后是出两个平行的业务层一个出可见框的结果一个出全身框的结果，但是两者的proposal或者anchor是一摸一样的！假设目前对于同一个标注的gt，可见框为Box<sub>visible</sub>,全身框为Box<sub>full body</sub>,那么对于个给定的proposal P, 当IoU(P, Box<sub>full body</sub>) > $\alpha$ && IoB(P, Box<sub>visible</sub>) >  $\beta$ 是P为正样本，否则P就是负样本。
  2. 因为两个branch用的是同一个proposal，然后基于这个proposal去分别回归offset，那么最后在后处理的时候这个proposal就只能有一个score，作者给了三个方法，第一只考虑可见分支，fc出的结果来个softmax就好，第二是只考虑全身分支，同样fc出的结果来个softmax就好，第三就是融合两个分支的结果把两个fc的结果对应相加然后再接个softmax就好，这样类似Bagging的做法来增强模型的鲁棒性。
  3. 论文另外一个需要注意的就是训练的细节了，全身框那个分支的回归就是和一般的Faster RCNN一样，直接只回归正样本的offset，但是对于可见框的回归是同时回归正样本和负样本的。这一点还是比较make sense的，因为论文中需要同一个proposal去同时回归全身框和可见框，所以对于那些高度遮挡的case，正样本proposal会比较少，反而是无遮挡或者轻微遮挡的case因为可见框和全身框的Overlap很大一般不会有明显的影响，因此这就会导致最后模型两个分支学的东西几乎是一样的。那么对于可见框分支对负样本怎么学习呢？作者将其学习目标定义为这个proposal的中心，所以它的gt为(0, 0, -INF, -INF)（实际使用的时候是(0, 0, -3, -3)），因为是log-space所以是-INF，这个可以看论文，这样就可以显示的将遮挡情况让模型进行感知。感觉这个做法会对FP的处理会比较有帮助。
