---
title: 'S3FD: Single Shot Scale-invariant Face Detector'
date: 2019-02-28 20:18:26
tags:
  - Object Detection
---
URL: http://openaccess.thecvf.com/content_ICCV_2017/papers/Zhang_S3FD_Single_Shot_ICCV_2017_paper.pdf
ICCV2017的一篇论文，主要研究小人脸的检测，作者针对目前基于anchor的检测器对小人脸表现不好的现象分析了几个可能的原因，并针对性的提出了具体的解决方法，比如新的anchor匹配策略、max out backgroud label等方法。

作者首先提出了目前小人脸的难点：
![](S3FD-Single-Shot-Scale-invariant-Face-Detector-屏幕快照 2019-03-02 上午10.08.37.png)
论文中也正是从这四点出发来解决小人脸的检测问题。

![](S3FD-Single-Shot-Scale-invariant-Face-Detector-屏幕快照 2019-03-02 上午10.07.42.png)
上图是论文中提出的基于VGG16的检测模型，比较特别的地方是Normalization Layer、conv3_3输出结果为Nm+4 而不是一般的2 + 4 以及 anchor设计的策略：
+ Normalization Layer是考虑到Conv3_3 - Conv5_3 feature scale差异比较大，所以对activation做了norm来加速训练。
+ Conv3_3作为最底层的detection layer，主要负责小人脸的检测，对于小人脸的检测通常需要设置比较多的anchor，这就会导致比较验证的正负样本不均衡的现象，于是作者就提出了max-out backgroud label的逻辑，cls的那个分支会预测N + 1个类别，1就是face，N就是backgroud，然后取最大的backgroud的score去参与计算loss以此来提高cls分支的分类能力，毕竟background归为一类很难去精确分类。
max out backgroud逻辑：
![](S3FD-Single-Shot-Scale-invariant-Face-Detector-屏幕快照 2019-03-02 上午10.19.42.png)
+ 至于anchor的设计策略，可以细看论文中的table：
![](S3FD-Single-Shot-Scale-invariant-Face-Detector-屏幕快照 2019-03-02 上午10.09.36.png)
anchor / stride 恒为4， 这样做的好处就是不同scale保证采样密度一致（两个anchor之间的overlap都是1/4anchor的大小）从而不同的人脸能基本匹配相同数目的anchor。而anchor具体的scale设置也是和一些观察经验有关的，比如论文提到的erf，有效感受野，因为对于图片的不同位置可以理解为权重是不一样的，比如靠近图片的中心他会有很大概率被其他的kernel重复计算，而边缘的pixel则相对被更加稀疏的计算，所以论文提到了ERF的概念，anchor也是针对ERF设计的：
![](S3FD-Single-Shot-Scale-invariant-Face-Detector-屏幕快照 2019-03-02 上午10.09.40.png)
因为anchor的匹配是根据IoU来的，那么对于一些face还是会不可避免的匹配不上anchor，所以论文就提出了一个补充匹配的逻辑来缓解这个问题，方法也很简单，用基本的匹配逻辑匹配完一轮之后，对于剩下没有匹配的那些小脸取IoU 大于0.1的anchor，排序取top N补充进来。这个想法相对比较直观吧。
具体的实验结果：
![](S3FD-Single-Shot-Scale-invariant-Face-Detector-屏幕快照 2019-03-02 上午10.14.05.png)
