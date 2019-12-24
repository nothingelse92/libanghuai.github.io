---
title: High Performance Visual Tracking with Siamese Region Proposal Network
date: 2019-01-15 22:50:13
tags:
  - Tracking
---
URL: http://openaccess.thecvf.com/content_cvpr_2018/CameraReady/2951.pdf
Siamese-RPN, CVPR2018一篇关于tracking的论文，论文所提方法的整个逻辑还是基于孪生网络来做，整体也可以理解为对Siamese-FC结构的改进，下图是Siamese-RPN的整个Pipeline：
![](High-Performance-Visual-Tracking-with-Siamese-Region-Proposal-Network-屏幕快照 2019-01-15 下午11.31.31.png)

Siamese-RPN网络的整个逻辑还是分成两个分支, 上面一个分支针对关键帧，下面一个分支针对检测帧，两个分支本身的参数是共享的，Siamese网络完成对两张输入图片的特征提取，两个分支的输出还会经过上图的Region Proposal Network网络来整合两者的信息.
具体做法和RPN网络类似，前一层Siamese网络的每一个分支都会接入两层的conv然后分成两个分支，一支为cls，另一支为reg，关键帧的输出如上图的4×4×(2k×256) 和 4×4×(4k×256)，这两个输出会分别作为对应的检测帧输出的kernel来进行卷积运算（上图中的*号操作）。这两者最后的输出就是检测帧最后的检测结果。
那么在得到最后预测的结果时针对tracking的应用场景，作者也提出了一些选框的策略，一是默认物体的移动速度不快，所以只选取靠近中心的anchor：
![](High-Performance-Visual-Tracking-with-Siamese-Region-Proposal-Network-屏幕快照 2019-01-20 上午10.43.46.png)
另外一个做法就是对当前得到的proposal进行重新排名，比如利用cosine window来对距离重点点比较远的候选框进行抑制或者同样基于物体移动速度不快的假设可以默认物体的size变化也不大所以对scale进行penalty，形变比较大的得分会被抑制，具体打分如下，k为超参数，r为proposal的ratio，r’为上一帧的ratio，s和s’则是对应的整体的ratio（输入图片多尺度scale），论文中应该采取的是后者：
![](High-Performance-Visual-Tracking-with-Siamese-Region-Proposal-Network-屏幕快照 2019-01-20 上午10.48.02.png)
**实验结果**
VOT2015:
![](High-Performance-Visual-Tracking-with-Siamese-Region-Proposal-Network-屏幕快照 2019-01-20 上午10.53.58.png)
VOT2016:
![](High-Performance-Visual-Tracking-with-Siamese-Region-Proposal-Network-屏幕快照 2019-01-20 上午10.55.21.png)
![](High-Performance-Visual-Tracking-with-Siamese-Region-Proposal-Network-屏幕快照 2019-01-20 上午10.55.25.png)
SiamRPN网络相比SiamFC直接出了框的具体坐标位置，感觉对于跟踪的任务更加合理，也避免了SiamFC相对比较繁琐的后处理，在测试集上的效果也是要比SiamFC好一些。
