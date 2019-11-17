---
title: Tone Mapping
date: 2019-06-20 11:47:44
tags:
  - Basic
---
Tone Mapping 可以简单理解为将HDR（High Dynamic Range） 的图像映射到LDR（Low Dynamic Range）的图像中，DR的定义可以理解为同一张图片中所有像素点最大亮度和最小亮度的log的差值，RGB场景下亮度的定义为：
```
 Y = 0.2126R + 0.7152G + 0.0722B
```
那么很显然在LDR下DR的取值只能到2.4，而在真实场景中DR的值甚至可以达到9以上.
针对Tone Mapping的操作也衍生出了很多优化的方法，比如[这篇论文](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.352.2669&rep=rep1&type=pdf)
这篇论文的应用场景是视频的压缩，那么针对这个场景常用的Tone Mapping逻辑一般如下， l为输入的HDR原图，θ为Tone Mapping操作的参数，v为HDR图经过Tone Mapping之后得到的LDR图, v~ 为v经过视频压缩、解压缩之后得到的LDR图，l~为v~经过Tone Mapping逆过程得到的伪HDR的图，所以将l和l～的diff作为整个优化过程的object function来优化θ的参数即可，比如来优化这两个值的MSE：
![](Tone-Mapping-屏幕快照 2019-06-21 上午10.28.40.png)
而本论文中提出的方法则是完全简化了这个过程，所以整个论文的核心就落到了distortion model的构建中：
![](Tone-Mapping-屏幕快照 2019-06-21 上午10.51.17.png)
求解distortion model的核心又是tone curve，类似直方图均衡的方法，为和人类视觉系统的感官逻辑保持一致，tone curve histogram的计算是基于HDR图的log10(亮度)来计算的：
![](Tone-Mapping-屏幕快照 2019-06-21 上午10.53.27.png)
整个curve是分段的线性映射，每一段的宽度都为δ （论文中取的0.1），横坐标可以理解为HDR域的情况，纵坐标可以理解为映射到LDR域的情况，每一段curve的计算，s<sub>k</sub>为斜率：
![](Tone-Mapping-屏幕快照 2019-06-21 上午10.54.40.png)
![](Tone-Mapping-屏幕快照 2019-06-21 上午11.08.03.png)
因为curve是分段线性的，所以tone mapping的逆向公式也就很好计算了：
![](Tone-Mapping-屏幕快照 2019-06-21 上午11.08.26.png)
单看论文中提出的简化的tone mapping计算方法和原始方法的最主要差别就是把压缩损失那一部分给去掉了，而实际作者是用了一个分布函数来简化了这个操作p<sub>C</sub>就是压缩损失的分布函数,p<sub>L</sub>实际就是histogram计算出来的比率了：
![](Tone-Mapping-屏幕快照 2019-06-21 上午11.09.13.png)
作者通过层层推导之后把上述的公式简化到：
![](Tone-Mapping-屏幕快照 2019-06-21 上午11.12.04.png)
![](Tone-Mapping-屏幕快照 2019-06-21 上午11.12.39.png)
然后利用Karush-Kuhn-Tucker (KKT)方法来优化计算得到s数组的取值，这样就可以直接求解整个tone mapping的逻辑了，至于具体的kkt方法有兴趣的同学可以自行去查找了或者直接refer论文去看细节。
这个方法最后的效果还是很惊艳的。
