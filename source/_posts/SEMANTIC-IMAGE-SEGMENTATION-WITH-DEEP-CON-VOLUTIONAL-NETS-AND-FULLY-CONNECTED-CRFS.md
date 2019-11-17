---
title: >-
  SEMANTIC IMAGE SEGMENTATION WITH DEEP CON- VOLUTIONAL NETS AND FULLY CONNECTED
  CRFS
date: 2019-01-24 20:51:46
tags:
  - Segmentation
  - DeepLab
---
URL:https://arxiv.org/pdf/1606.00915.pdf
Semantic Segmentation比较经典的DeepLab系列的第一篇，DeepLab V1，主要利用FCN和DenseCRF来实现比较出色的segmentation效果，DeepLab V1的整个Pipeline整体可以分成两个部分: FCN得到相对比较粗糙的分割结果，DenseCRF在FCN结果的基础上对边缘进行Refine得到相对比较分明的物体分割轮廓。
在FCN部分主要是基于VGG16，将VGG16中FC换成Conv变成全卷积网络，而为了保持相对比较dense的feaure map,从VGG16原来的32倍下采样提高到8倍下采样，这个可以通过改变stride再加上空洞卷积来保持感受野，空洞卷积的具体示意图如下，网上也可以找到比较详细的解释：
![](SEMANTIC-IMAGE-SEGMENTATION-WITH-DEEP-CON-VOLUTIONAL-NETS-AND-FULLY-CONNECTED-CRFS-屏幕快照 2019-01-24 下午8.53.19.png)
最后在inference的时候可以直接从8倍下采样的结果直接插值回原图resolution.

第一阶段FCN得到的pixel wise的结果无疑是相对比较粗糙的，论文中作者也给了一些对比的图,FCN的结果相比较gt还是有比较大的差距的：
![](SEMANTIC-IMAGE-SEGMENTATION-WITH-DEEP-CON-VOLUTIONAL-NETS-AND-FULLY-CONNECTED-CRFS-屏幕快照 2019-01-24 下午9.21.37.png)
论文中提及的FC CRF本身是针对轮廓的处理提出来的，论文中也给出了具体的计算公式，x<sub>i</sub>为pixel的label，I为pixel的颜色轻度，整体逻辑是基于pixel的位置和pixel的颜色强度尽量让“相似”的pixel归为一类，让“不同”的pixel归为另一类，这样就可以在物体的边缘区域有一个比较好的划分：
![](SEMANTIC-IMAGE-SEGMENTATION-WITH-DEEP-CON-VOLUTIONAL-NETS-AND-FULLY-CONNECTED-CRFS-屏幕快照 2019-01-24 下午9.29.59.png)
![](SEMANTIC-IMAGE-SEGMENTATION-WITH-DEEP-CON-VOLUTIONAL-NETS-AND-FULLY-CONNECTED-CRFS-屏幕快照 2019-01-24 下午9.30.02.png)
## Experiments
在VOC2012上的测试结果，MSc代表多尺度的特征融合，论文中是对网络的最后4个mas pooling层进行特征融合，FOV则是和空洞卷积有关：
![](SEMANTIC-IMAGE-SEGMENTATION-WITH-DEEP-CON-VOLUTIONAL-NETS-AND-FULLY-CONNECTED-CRFS-屏幕快照 2019-01-24 下午9.38.17.png)
作者也过空洞卷积的参数设置进行了简单的实验：
![](SEMANTIC-IMAGE-SEGMENTATION-WITH-DEEP-CON-VOLUTIONAL-NETS-AND-FULLY-CONNECTED-CRFS-屏幕快照 2019-01-24 下午9.40.38.png)
![](SEMANTIC-IMAGE-SEGMENTATION-WITH-DEEP-CON-VOLUTIONAL-NETS-AND-FULLY-CONNECTED-CRFS-屏幕快照 2019-01-24 下午9.41.21.png)
