title: 'Pelee: A Real-Time Object Detection System on Mobile Devices'
tags:
  - Object Detection
author: libanghuai
date: 2018-09-25 20:05:00
---
这篇论文主要做的内容是在手机终端上进行实时的物体检测并提出了PeleeNet模型，不同于目前的MobileNet、ShuffleNet等基于 depthwise separable convolution的模型，PeleeNet是基于传统的卷积来实现的，因为作者认为depthwise separable convolution操作在诸多的框架下都没有有效的支持……
论文主要借鉴了DenseNet、DSOD、Inception-V4等现有的网络，模型结构在论文中给出了比较详细的列表，几个需要注意的细节：
1. 作者参考GoogleNet在DenseBlock中引入2-way dense layer来获得不同大小的感受野。

![](Pelee-A-Real-Time-Object-Detection-System-on-Mobile-Devices-image004.png)

2. DenseNet中表现较好的DenseNet-BC结构是在原有的基本DenseNet结构基础上加入了Bottleneck和Compression，但是作者认为compression逻辑的加入影响模型的特征表达，所以下表中的Transition Layer没有compression的逻辑，输入输出channel数保持一致。

![](Pelee-A-Real-Time-Object-Detection-System-on-Mobile-Devices-image002.png)

3. Composite Function中用Post-activation替换掉DenseNet中的Pre-activation，以此来在inference阶段加速计算。
4. 用Bottleneck Layer控制输出的channel数不大于输入的channel数，此操作除了可以节省28.5%的计算量，对准确率的影响也比较小。

![](Pelee-A-Real-Time-Object-Detection-System-on-Mobile-Devices-image003.png)
