---
title: Object as Distribution
date: 2020-01-15 22:29:40
tags:
  - Object Detection
---
URL: https://arxiv.org/abs/1907.12929

NIPS 2019的一篇论文，论文的内容还是很新颖的，在检测领域传统的物体表示是通过框来表示，那么这种表示有一个很大的弊端是结合后处理NMS针对crowd的场景几乎是无解的，所以作者提出一个思考用一个框来定位一个物体是不是合理的一种表示方式，因此论文中作者提出了用分布来标志物体，作者给的几个sample还是比较有意思的，涵盖了作者claim的crowd的场景：

![](Object-as-Distribution-屏幕快照 2020-01-15 下午10.33.53.png)

具体的话论文中选择用二元正态分布来描述一个物体，公式没什么特别的就是标准的二元正态分布的定义:

![](Object-as-Distribution-屏幕快照 2020-01-20 下午9.57.09.png)

从公式中我们也可以看到，那么网络需要学习的有5个参数: μ<sub>xi</sub>,μ<sub>yi</sub>,σ<sub>xi</sub>,σ<sub>yi</sub>,ρ<sub>i</sub>, 这五个参数刚好可以恢复出一个分布出来，比如 μ<sub>xi</sub>、μ<sub>yi</sub>两个参数起码已经恢复出物体的中心来了,σ<sub>xi</sub>、σ<sub>yi</sub> 这两个参数又直接和形状相关，所以用分布来表示物体本身就是make sense的，同时因为分布可以严格区分开物体的中心，从逻辑上讲是有利于解遮挡的场景的。那么为了让学习的目标是和位置无关的，对于具体的学习目标作者进行了转换：m−μ<sub>xi</sub>,n−μ<sub>yi</sub>,logσ<sub>xi</sub>,logσ<sub>yi</sub>,tanh<sup>−1</sup>ρi,(m,n)代表一个处于某个物体内的pixel坐标。

那么接下来就说说如何来优化训练模型，首先对于分布来说自然而然就想到用KL散度来监督两个分布之间的距离，同时论文中基于DeepLab的框架选择以联合训练的方式来训练模型，那么总的监督loss就是L<sub>seg</sub> + L<sub>KL</sub> + L<sub>cls</sub>, 那么在具体训练的时候为了加速训练以及节省资源，作者选择downsample之后监督而不是再resize，所以inference的时候就需要有一个upsample的过程，这个过程无法对边缘点有一个很好的判断，有可能会把边缘点误判为另一个分布，所以作者借助(Mixure Density Network<sub>暂时没读过这篇论文</sub>)以一种bagging的逻辑对于一个object预测多个distribution，所以那个L<sub>cls</sub>就来自于这。

另外需要注意的一个细节是L<sub>KL</sub>为：

![](Object-as-Distribution-屏幕快照 2020-01-20 下午10.56.04.png)

m<sub>rep</sub>为mask，只有当cls值最高或者KL loss最小的那个分布mask才为1否则都为0！用作者论文中的话说，模型希望最好的distribution(KL Loss最低和目前选择的distribtion(cls值最大)越接近目标distribution！

其他论文就没有啥注意的了，NMS最后用KL距离就好。
论文另外有一个比较大的槽点就是实际上最后的点很低！！！
