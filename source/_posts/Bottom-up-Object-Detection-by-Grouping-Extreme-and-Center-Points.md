---
title: Bottom-up Object Detection by Grouping Extreme and Center Points
date: 2019-03-02 11:04:25
tags:
  - Bottom_Up
  - Detection
---
URL:https://arxiv.org/pdf/1901.08043.pdf
Codebase:https://github.com/xingyizhou/ExtremeNet
一篇比较有意义的论文，主要是用bottom up的方法来做检测的问题，整个工作是基于ECCV2018的cornernet来做的，对于一个框ExtremeNet会出5个点，四个边界点和一个中心点，中心点主要是用来做group。下图是标注的示例图：

![](Bottom-up-Object-Detection-by-Grouping-Extreme-and-Center-Points-屏幕快照 2019-03-02 下午4.52.39.png)

论文的整体框架也很简单，主要都是基于CornerNet的code进行改的，对于一张图片出5个点的heatmap和四个offset的heatmap：
![](Bottom-up-Object-Detection-by-Grouping-Extreme-and-Center-Points-屏幕快照 2019-03-02 下午4.44.43.png)
当拿到最后的5点heatmap和4个offset之后，对于点的instance group方法也很简单甚至比较暴力，首先会设置一个阈值T<sub>p</sub>，那么4个边界点heatmap上大于T<sub>p</sub>的话就会被记为一个candidate，论文中也说了在 coco数据集上一般会有40个左右，那么匹配方法很简单，暴力O(N^4)轮询，然后对于当前的4个点，直接通过取平均的方式得到中心点，然后按这个中心点的位置去中心点的heatmap上取值，如果这个值大于某个阈值T<sub>c</sub>，那么就认为这是一个合法的框：
![](Bottom-up-Object-Detection-by-Grouping-Extreme-and-Center-Points-屏幕快照 2019-03-02 下午4.45.10.png)
这样利用bottom up进行detection任务的整个框架大体就是如此，那么这种方法也有两个比较明显的问题：
+ Ghost box：假设有三个一样大小、水平或者竖直等距离排布的物体，那么利用extremenet来预测的时候应该会出现4个比较高置信度的框！，因为外围两个框的四个边界点可以组成比较高置信度的框同时中心点还落在中间框的中心点会大概率满足阈值，论文中将这种box称之为ghost box，解决方法比较直接，如果出现一个框内部三个框的score之后大于本身这个大框('ghost box')score的3倍，那么就将这个大框的置信度/2，这样有助于在解析来的NMS中remove掉这个box。
+  Edge aggregation：假设对于方方正正的物体，比如汽车等，那么由于它的边界点并不是很唯一，它的整条边的点都可以作为边界点，所以后果就是整条边的confidence都不高影响最后的box的生成，因此论文中作者借鉴来cornernet中的pooling的一些想法，对给定的一个extreme point在水平和竖直两个方向以单调递减(heatmap的score)的方法一直遍历直到找到一个局部最小值，过程中遍历的点score的和会作为加权的一部分算到当前extreme point的置信度上：
单调递减过程中的点：
![](Bottom-up-Object-Detection-by-Grouping-Extreme-and-Center-Points-屏幕快照 2019-03-03 上午10.20.37.png)
extreme point score的计算：
![](Bottom-up-Object-Detection-by-Grouping-Extreme-and-Center-Points-屏幕快照 2019-03-03 上午10.20.32.png)
具体的case：
![](Bottom-up-Object-Detection-by-Grouping-Extreme-and-Center-Points-屏幕快照 2019-03-03 上午10.22.36.png)
作者同时也把这个方法应用到instance seg的任务中，只是标注略微粗糙利用一个八边形来做mask，具体细节不赘述，具体示例：
![](Bottom-up-Object-Detection-by-Grouping-Extreme-and-Center-Points-屏幕快照 2019-03-03 上午10.12.15.png)
**实验结果还是很不错的：**
![](Bottom-up-Object-Detection-by-Grouping-Extreme-and-Center-Points-屏幕快照 2019-03-03 上午10.12.06.png)
