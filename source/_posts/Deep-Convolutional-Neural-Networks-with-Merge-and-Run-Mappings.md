---
title: Deep Convolutional Neural Networks with Merge-and-Run Mappings
date: 2019-08-03 23:27:20
tags:
  - Basemodel
---
URL: https://www.ijcai.org/proceedings/2018/0440.pdf

MSRA wangjingdong组的研究工作，貌似虽然中了2018年的IJCAI但是实际上这篇论文应该挂出来蛮久了。
论文的主要内容也很直接，就是想训练更深、更宽的网络结构，并因此提出了Merge and Run模块：

![](Deep-Convolutional-Neural-Networks-with-Merge-and-Run-Mappings-截屏2019-12-2123.28.36.png)

Merge-and-Run模块其实和Inception-Resnet的结构有一点类似，Inception-Resnet结构在模块内是一个parallel的结构，Merge-and-Run模块本身也是两个分支parallel连接，只是不同于前者输入直接和输出sum，后者是把两个分支输入的均值分别和两个分支的输出sum，求均值和sum的操作分别被称作“merge”和 “run”, 本身结构的设计也有利于深度网络的学习,
![](Deep-Convolutional-Neural-Networks-with-Merge-and-Run-Mappings-thumbnail_image004.png) = M是幂等矩阵和resnet的恒等变换类似有助于深度网络的训练:

![](Deep-Convolutional-Neural-Networks-with-Merge-and-Run-Mappings-截屏2019-12-2123.30.00.png)

作者也论证了这样的结构在保证相同数目结构单元的情况下比resnet、inception-restnet这样的结构更浅、更宽，因此也适合训练很深的网络结构：

1. 更浅：假设每一个block（上图中的1组conv）有B个layer，三个结构总的block数分别是2L、L、L个，那么对于一般的resnet结构平均深度就为BL + 2，resnet-inception结构平均深度为2BL/3 + 2，而本文提出的merge-and-run结构平均深度就为BL/3+2，+2为上图输出的FC和输入的conv，至于平均深度的计算个人理解就是block数除以可能的path数。

2. 更宽：对于merge-and-run模块宽度更宽的论证论文做了简单的两组论证，都比较直观，inception-resnet parallel的结构比普通的resnet有更多的分支，merge-and-run与inception-resnet的差别是inception-resnet最后的结果会融合在一起而merge-and-run 最后的结果还会是两个分支：

![](Deep-Convolutional-Neural-Networks-with-Merge-and-Run-Mappings-截屏2019-12-2123.32.04.png)
