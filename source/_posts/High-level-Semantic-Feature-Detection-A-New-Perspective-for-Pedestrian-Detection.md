---
title: >-
  High-level Semantic Feature Detection: A New Perspective for Pedestrian
  Detection
date: 2019-12-22 16:30:45
tags:
  - Pedestrian Detection
  - Face
  - Object Detection
---
URL: http://openaccess.thecvf.com/content_CVPR_2019/papers/Liu_High-Level_Semantic_Feature_Detection_A_New_Perspective_for_Pedestrian_Detection_CVPR_2019_paper.pdf
这是CVPR2019的行人检测论文，和Center and Scale Prediction: A Box-free Approach for Object Detection是同一篇论文，倒是后者的标题更能体现出论文所提的方法，论文主要就是异于Anchor Based的方法提出了预测中心点+框的scale的新方法来解决行人检测的问题或者严格来讲解决一般刚性物体的检测问题. 从行人检测的角度来说是一个比较新颖也比较值得去思考的方法。下图是它整体的Pipeline：

![](High-level-Semantic-Feature-Detection-A-New-Perspective-for-Pedestrian-Detection-屏幕快照 2019-12-22 下午4.35.00.png)

论文所提的方法三言两语倒是可以说出大概，但是一些细节还是值得去琢磨的：
1. 模型结构很简单，Res50，stage 2 - 5通过deconv将高层feature map统一到一个固定的resolution然后concat到一起，其实比较类似FPN了，r倍downsample之后接一层Conv3x3然后再分别接两个Conv1x1引出两个branch，一个预测中心点，一个预测Scale，两者都是用heatmap出.
2. 模型结构很简单，比较重要的就是gt是怎么生成的以及如何去监督了，首先说center的heatmap，它的大小（w x h）和downsample之后的feature map维度是一致的, 给定一个bounding box可以得到一个标注的中心，那么原则上只有这个点的gt为1，其余位置gt都为0，那么论文中为了更加符合实际场景同时也更加便于训练，用了一个比较常用的高斯分布来生成gt。而至于scale map，对于给定的中心点，它的取值是log(h),h为boundingbox的高度，这样可以通过事先定义的ratio反算出具体的框。同样的在实际生成gt的时候会围绕中心点半径2的范围内都置为log(h)其余为0；
3. Loss方面center的定位可以看为classification任务所以用ce loss，而scale的预测可以理解为regression任务所以用smoothL1 loss。
4. 因为gt的监督是基于r倍downsample的所以为了更精确的预测会自然想到加一个offset分支来更精确的回归：
  L = λ<sub>c</sub>L<sub>center</sub> + λ<sub>s</sub>L<sub>scale</sub> + λ<sub>o</sub>L<sub>offset</sub>

最后就是这篇论文刷的点还是比较高的，也会是做检测比较好的一个方向。
