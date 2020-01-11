---
title: Single-Shot Refinement Neural Network for Object Detection
date: 2018-09-24 18:10:10
tags:
  - Object Detection
---
论文致力于研究整合two-stage检测器和one-stage检测器的优点提出了RefineDet网络结构。

![](Single-Shot-Refinement-Neural-Network-for-Object-Detection-image003.png)

网络结构整体借鉴了SSD模型并结合了cascade逻辑，整个网络主要分成三个部分：
1. Anchor  Refinement Module（ARM）：这一模块的模型结构和SSD比较类似，不同的是它只做二分类，ARM对anchor进行初步的refine并得到相对粗糙的位置信息和分类信息，这一部分信息将作为ODM模块的输入。
特别的，为了缓解类别失衡问题，论文提出了Negative Anchor过滤逻辑，将negative confidence大于某一个阈值（如0.95）的anchor全部舍弃掉不送入ODM模块。
2. Object Detection Module (ODM): 这一模块的结构和ARM类似，并且两者对应layer的size是一致的，论文中给出的结构图例比较直观。ODM模块主要是借助ARM模块refine过后的anchor进行进一步细致的处理并最终输出物体的类别信息和bounding box的位置信息。在具体实现的时候，ARM和ODM之间采用了类似FPN特征融合的方法将ARM高层的feature map与低层的feature map相融合并与ODM对应layer的feature map一同作为下一层layer的输入，以此来提高检测的效果。
3. Transfer Connection Block (TCB)：TCB主要用来融合低层和高层feature map，论文给的图示比较清楚主要就是对高层feature map进行deconv并与低层feature map相加；

![](Single-Shot-Refinement-Neural-Network-for-Object-Detection-image002.png)

实验结果：

![](Single-Shot-Refinement-Neural-Network-for-Object-Detection-image004.png)
