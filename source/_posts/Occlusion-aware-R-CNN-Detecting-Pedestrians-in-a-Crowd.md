---
title: Occlusion-aware R-CNN - Detecting Pedestrians in a Crowd
date: 2019-12-08 22:17:25
tags:
  - Pedestrian Detection
---
URL:http://openaccess.thecvf.com/content_ECCV_2018/papers/Shifeng_Zhang_Occlusion-aware_R-CNN_Detecting_ECCV_2018_paper.pdf

这篇论文算是去年去参加ECCV2018 poster展台最火的一个了，论文同样是做遮挡场景的行人检测的，方法也是花样attention。

论文同样基于faster rcnn的框架来做行人检测，作者所提方法有两个核心，一个是**Part Occlusion aware RoI Pooling Unit**，作者把人的body分成5个part分别过ROI Pooling得到固定的输出大小，论文中是7x7，然后经过**Occlusion process unit**得到每个part的可见与否的置信度c，c再分别和对应的part feature相乘得到加权后的feature，最后连同body本身大part的feature通过element wise sum得到最后attention的feature：

![](Occlusion-aware-R-CNN-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-0822.21.19.png)

论文所提方法的另外一个核心是**Aggregation Loss**，这玩意其实和**Repulsion Loss**这篇论文的理论算是很像的了，核心想法是希望同一个gt的proposals(anchors)之间需要尽可能的近:

![](Occlusion-aware-R-CNN-Detecting-Pedestrians-in-a-Crowd-截屏2019-12-0822.34.14.png)

{t<sub>1</sub><sup>\*</sup>, t<sub>2</sub><sup>\*</sup>...}是涉及多个proposal（anchor）的gt集合，集合长度为ρ，{Φ1, · · · , Φρ}是上述对应的gt相对应的anchors的集合，公式写的其实很直白不赘述。
