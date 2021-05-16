---
title: Non-local Neural Networks
date: 2020-06-07 21:43:46
tags:
  - Obejct Detection
  - Classification
  - Basemodel
  - Classic
---
URL:https://arxiv.org/abs/1711.07971

CVPR2018的论文，比较经典的论文，也是目前经常被使用的方法。方法的核心是不局限于目前的local操作而是提供了一种long range的解决方案。

先看下论文中对non-local公式性的定义:

![](Non-local-Neural-Networks-截屏2020-06-07 22.30.18.png)

其中，x是输入feature map, 比如大小为c x h x w的输入，x<sub>i</sub>为h x w平面上的一个点，y<sub>i</sub>为其对应的输出，x<sub>j</sub>是x<sub>i</sub>的邻居，c则是用来归一化的，f为计算平面上两点x<sub>i</sub>和x<sub>j</sub>的相似度函数，g为一个encoding函数，可以理解为对x<sub>j</sub>对应的feature进行encoding的函数。为了简单起见论文里用1x1的conv来实现g函数，而对于f函数论文里提供了比较多的proposal:
Gaussian:

![](Non-local-Neural-Networks-截屏2020-06-07 22.35.29.png)

Embeded Gaussian,θ(x<sub>i</sub>) = W<sub>θ</sub>x<sub>i</sub> and φ(x<sub>j</sub>) = W<sub>φ</sub>x<sub>j</sub>, 论文里最后也用1x1实现了这两个embedding函数:

![](Non-local-Neural-Networks-截屏2020-06-07 22.36.05.png)

Dot Product:

![](Non-local-Neural-Networks-截屏2020-06-07 22.37.58.png)

Concat:

![](Non-local-Neural-Networks-截屏2020-06-07 22.38.28.png)

***其中Embeded Gaussian需要特别注意，如果取c为f的和，那么non-local的前半部分：1 / c * sum(f(xi, xj)) 就是标准的softmax操作！！***

那么最后我们来看下如何在标准的CNN网络中实现non-local模块, 这里为了保证non-local的即插即用性作者将其设计为一个residual的方式：

![](Non-local-Neural-Networks-截屏2020-06-07 22.42.48.png)

下图是具体的结构, 应该很好看懂，实现的很巧妙，最上面的那个乘号实际上就是non-local的表现形式:

![](Non-local-Neural-Networks-截屏2020-06-07 22.10.18.png)
