---
title: 'Pose-Invariant 3D Face Alignment '
date: 2019-05-01 22:42:04
tags:
  - Landmark
  - 3D
---
URL:https://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Jourabloo_Pose-Invariant_3D_Face_ICCV_2015_paper.pdf
利用3D模型来处理大Pose Landmark问题的一篇文章，整体还是follow 3DMM的那一套pipeline，只是因为这篇论文出的相对比较早，对于参数的学习是利用一般的回归模型来级联回归，整体在不同Pose下还是有一定效果的。不过和整个pipeline的设计有关速度比较慢。

下图是论文中描述的整体Pipeline：
![](Pose-Invariant-3D-Face-Alignment-屏幕快照 2019-04-24 下午11.31.33.jpg)
整体可以分为这么几个主要部分：
**第一是3D模型的抽象**
这一部分和3DMM是一致的，任意人脸的描述被定义成平均脸和delta的和：
![](Pose-Invariant-3D-Face-Alignment-屏幕快照 2019-05-29 下午10.43.41.png)
3D Landmark到2D Landmark的映射被定义为U = MS, 其中M为弱视角投影的参数.所以求解人脸的3D模型就会被转化成参数P = {M,p}的求解
那么通常我们对3D/2D人脸的标注只会涉及到Landmark点的坐标,没有办法得到M和p的gt，所以论文中也具体说明了M和p的gt的生成。首先定义具体的目标函数，那么对于理想的gt函数J应该为0，所以只要想办法最小化这个函数就行，具体计算的时候实际上是采用了类似启发式规则的方法，先让p为0，然后去求最优的M，然后再固定M去求最优的P，以此逐步迭代直到前后两次的值之间的delta很小，那么这个时候的M和p就是最后的gt，（V是关键点的可见性标注）:
![](Pose-Invariant-3D-Face-Alignment-屏幕快照 2019-05-29 下午10.44.07.png)
**第二部分就是具体的模型训练**
关于具体参数的学习论文中所提的方式是在cascade的每一个阶段都出两个回归模型来分别回归M和p, 特征的提取是用的HOG特征，其他似乎没有什么特殊的:
![](Pose-Invariant-3D-Face-Alignment-屏幕快照 2019-05-29 下午10.44.42.png)
论文中所提模型同时还出了点的可见性与否，但是实际上在预测的时候点的可见性是算出来的而不是模型直接预测出来的…具体可以看论文中的详细说明
![](Pose-Invariant-3D-Face-Alignment-屏幕快照 2019-05-29 下午10.45.07.png)
综合整个Pipeline的逻辑：
![](Pose-Invariant-3D-Face-Alignment-屏幕快照 2019-05-29 下午10.45.40.png)
至于在Benchmark上的结果，对于不同的Pose效果还是有的，不过在AFW上整体似乎差于TCDCN：
![](Pose-Invariant-3D-Face-Alignment-屏幕快照 2019-05-29 下午10.46.06.png)
这篇论文另一个比较有趣的现象就是当对每一个landmark点进行分别评估NME的时候会发现面部的边界点误差很大，几乎是其他关键点的2倍，我们最近在做的时候也发现有同样的问题，因为面部边界很难有明确的语意定义，所以这个现象很难避免，对于这个问题CVPR2019的一篇semantic alignment论文还是很有参考意义的：
![](Pose-Invariant-3D-Face-Alignment-屏幕快照 2019-05-29 下午10.46.31.png)
