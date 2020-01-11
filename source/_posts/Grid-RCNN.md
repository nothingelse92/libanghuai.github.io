---
title: Grid RCNN
date: 2019-03-15 13:45:59
tags:
  - Object Detection
---
URL:https://arxiv.org/abs/1811.12030
CVPR2018的一篇论文，从某种程度上来说是借鉴Bottom Up的方法来优化目前检测方面的一些问题，主要出发点还是希望检测器出的框能尽可能的准，所以相比较一般的检测器直接出四维的坐标信息，Grid RCNN则是出9个点，用9个点的信息来表示一个bbox。
具体的PipeLine如下：
![](Grid-RCNN-屏幕快照 2019-03-16 下午3.04.35.png)

![](Grid-RCNN-屏幕快照 2019-03-16 下午3.04.42.png)
Grid RCNN本身是基于RCNN这一套Two Stage的逻辑来做的，所以相比较Faster RCNN主要就是Fast RCNN那个分支做了一些优化，主要几个方面：
1.  **Grid Guided Localization**
用NxN个均匀的点来表示一个框而不再是直接回归两个顶点坐标，这样做相比较FC回归点的好处是Conv保留了物体的一些空间位置信息，有助于物体的定位，而类似的 Grid RCNN相比较CornerNet之类基于脚点的检测模型好处在于CornerNet是直接出两个顶点的信息，但是实际上对于一个框的两个顶点它实际上多数处在一个backbroud上，实际可利用的有价值的信息很有限，因此Grid RCNN以及ExteamNet实际上在一定程度上都缓解了这个问题，那么通过Heatmap得到NxN个点之后(共NxN个Heatmap)就可以通过简单的坐标转换得到在原图中NxN个点的坐标，而将NxN个点转换称框的时候作者也提出了自己的逻辑：
（ (Px,Py) is the position of upper left corner of the proposal in input image, wp and hp are width and height of proposal, wo and ho are width and height of output heatmap）
![](Grid-RCNN-屏幕快照 2019-03-16 下午3.11.08.png)
本质是用四条边上N个点坐标的加权平均作为边的坐标：
![](Grid-RCNN-屏幕快照 2019-03-16 下午3.11.11.png)
2.  **Grid Points Feature Fusion**
这一部分可以理解为对Grid RCNN的优化了，作者认为NxN点之间是存在比较强的关联信息的，点与点之间相辅相成可以达到共同促进的作用，所以提出了fusion的逻辑：
![](Grid-RCNN-屏幕快照 2019-03-16 下午3.14.19.png)
做法也很粗暴直接，就是直接将最近点的heatmap通过**3层5x5的conv提取特征**之后直接和当前点的heatmap**取sum**:
![](Grid-RCNN-屏幕快照 2019-03-16 下午3.17.14.png)
那么对于**最近点**的定义论文中也给的很清楚，比如距离为1，那就是相邻的所有点，距离为2，那就是所有距离当前点2个单位长度的点综合来fusion，上面的示意图给的比较清楚，对于当前点的**最近点**被定义为'source point'
3.  **Extended Region Mapping**
![](Grid-RCNN-屏幕快照 2019-03-16 下午3.19.26.png)
这个优化主要是针对RPN给出的Proposal不够准，导致定义的NxN个点其实并不能包含检测的物体，那么最简单的方法就是把proposal认为放大，但是这样人为放大之后会严重影响检测的效果，尤其是对小物体而言，所以作者认为考虑到整个CNN的运算过程中感受野是足够的，所以就可以把这些个proposal**看作**是4倍于原来Proposal大小,那么我们就需要直接改坐标的映射关系就好：
![](Grid-RCNN-屏幕快照 2019-03-16 下午3.23.47.png)
这个可以和上面提供的原公式做一个简单的化简其实就是在原来的基础上加了一个偏移量来smooth这个操作，其实整体感觉也好理解，虽然proposla给的框比较小，但是因为感受野的原因最后抽取的特征是可以包含object的信息的，所以就可以直接理解为这个点的坐标偏移相比正常的proposal来说更大，所以需要重新计算加一个偏移量。

结果上也是不错的：
![](Grid-RCNN-屏幕快照 2019-03-16 下午3.32.52.png)
