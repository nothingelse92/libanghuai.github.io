---
title: 'FCOS: Fully Convolutional One-Stage Object Detection'
date: 2020-01-03 22:02:37
tags:
  - Classic
  - Object Detection
---
URL: https://arxiv.org/pdf/1904.01355.pdf

2019年备受推崇的一篇anchor free的论文，最后也应该中了CVPR2019，但是对于熟悉检测领域的同学来说，看到这篇论文应该略有眼熟，这篇论文其实和Densebox应该属于一脉相承，都希望利用FCN的逻辑统一检测/分割等任务。

FCOS的具体想法呢是这样的，基于FPN的结构，P3 - P7得到的feature map假设分别是H<sub>i</sub> x W<sub>i</sub> x C<sub>i</sub>，那么基于H x W这么多个pixel，每个pixel都作为一个中心点去回归一个目标bbox，每个bbox的定义同样包含5个值，一个是分类score，一个是回归offset，只是这个offset的值当前这个pixel距离gt框四条边的具体(和anchor based模型一样，feature map的pixel gt的计算只需要按stride映射会原图就好)，具体的示意图如下：
![](FCOS-Fully-Convolutional-One-Stage-Object-Detection-屏幕快照 2020-01-06 上午12.20.12.png)
几个需要注意的点:
1. gt的生成，对于feature map上的某个pixel (x, y), 如果(x, y) 映射到原图的点(x<sup>'</sup>, y<sup>'</sup>)落在了某个gt框里，那么对应的offset就算(x<sup>'</sup>, y<sup>'</sup>)到gt框四条边的offset，分类gt也就沿用这个gt框的class标注。
2. FPN的好处一是可以fuse feature另外一个不同的layer可以针对性的回归不同size的框，那么在FCOS中这部分是怎么做的呢，论文将P3 - P7 5个FPN层用6个值进行区间划分 ，论文中是用的区间m = [0, 64, 128, 256, 512 , ∞]，对于一个gt: (l<sup>∗</sup>, t<sup>∗</sup>, r<sup>∗</sup> ,b<sup>∗</sup>),如果满足
max(l<sup>∗</sup>, t<sup>∗</sup>, r<sup>∗</sup> ,b<sup>∗</sup>) > m<sub>i</sub> or max(l<sup>∗</sup>, t<sup>∗</sup>, r<sup>∗</sup> ,b<sup>∗</sup>) < m<sub>i - 1</sub>，那么P<sub>3 + i</sub>就会将这个gt视为negative sample，这层不负责回归这个gt框

![](FCOS-Fully-Convolutional-One-Stage-Object-Detection-屏幕快照 2020-01-03 下午10.04.06.png)
