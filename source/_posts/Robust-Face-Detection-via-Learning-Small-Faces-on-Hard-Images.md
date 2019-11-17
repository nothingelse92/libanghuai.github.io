---
title: Robust Face Detection via Learning Small Faces on Hard Images
date: 2018-12-19 23:45:19
tags:
  - Face
  - Detection
---
URL: https://arxiv.org/pdf/1811.11662.pdf
论文主要是想解决人脸检测中的小脸问题，论文的motivation其实和SNIP很像，让网络去学习一个相对固定的scale，比如在本论文中anchor大小被设置为固定的16x16，32x32，64x64三个。论文主要的内容有两个部分，一部分是提出来的检测网络，另一部分就是hard image mining。

论文所提的检测网络backbone采用VGG16，在接入cls和reg前经过dilation conv来获得不同的感受野，其他没有特别的内容:
![](Robust-Face-Detection-via-Learning-Small-Faces-on-Hard-Images-9add084a4879a4cde2af6ba16849afa33dc4a245_1_690x283.png)
![](Robust-Face-Detection-via-Learning-Small-Faces-on-Hard-Images-e9d7b5e89400c3d6572e883406879031b064fcd8_1_482x500.png)
而hard image mining是image level的，作者针对图片的难易程度定义了一个变量WPAS, 简言之是综合proposal的IOU以及score信息来量化图片的难易程度，WPAS>0.85被定义为简单的图片：
![](Robust-Face-Detection-via-Learning-Small-Faces-on-Hard-Images-5d2afb9dc7cdd4a787fc08fb6b82bda9f305572f_1_690x106.png)
最后在训练的时候对于当前epoch会借助前一个epoch的信息把训练数据都划分成easy和hard两类，再从easy中以0.7的概率剔除掉一些图片，剩下的图集作为当前epoch的训练数据来训练模型。
