---
title: Self-Challenging Improves Cross-Domain Generalization
date: 2020-07-10 12:07:15
tags:
  - Domain Adaptation
  - Classification
---
URL: https://arxiv.org/pdf/2007.02454.pdf

这是ECCV 2020的一篇Oral，算是从一个新的角度去做Domain Adaptation, 但是方法不是很新了，之前有挺多类似的工作了。

![](Self-Challenging-Improves-Cross-Domain-Generalization-屏幕快照 2020-07-10 下午12.24.48.png)

文中上述的一张图可以说明问题，做法就是在模型训练过程中丢掉那些Gradient比较大的feature：

![](Self-Challenging-Improves-Cross-Domain-Generalization-屏幕快照 2020-07-10 下午12.26.04.png)

做法可以联想到dropout和dropblock，论文比较多的篇幅是在理论证明，比较难懂，有兴趣可以看原文，最后在ablation的时候作者也比较了一下不同的drop的方式，随机drop的结果也还可以：

![](Self-Challenging-Improves-Cross-Domain-Generalization-屏幕快照 2020-07-10 下午12.27.19.png)

一些benchmark的点：

![](Self-Challenging-Improves-Cross-Domain-Generalization-屏幕快照 2020-07-10 下午12.27.58.png)
