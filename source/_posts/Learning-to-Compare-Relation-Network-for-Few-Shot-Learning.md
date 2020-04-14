---
title: 'Learning to Compare: Relation Network for Few-Shot Learning'
date: 2020-04-14 17:49:55
tags:
  - Few Shot
  - Classification
---
URL: http://openaccess.thecvf.com/content_cvpr_2018/papers_backup/Sung_Learning_to_Compare_CVPR_2018_paper.pdf

CVPR2018的论文，整体pipeline很简单，做法也比较make sense，感觉比较不错的一篇论文，论文解决few shot的出发点是构建test image和support image之间的关系来给test image赋label。本质上和那些基于最近邻的逻辑是一样的，只是它把这个compare的过程放到网络里显式的去学习了。


![](Learning-to-Compare-Relation-Network-for-Few-Shot-

Learning-屏幕快照 2020-04-14 下午5.57.44.png)
上面这张图其实画的挺清楚的（5 way 1 shot），整个pipeline主要有两个模块，一个embedding module，一堆conv对image抽一下特征，另一个是relation module，这个主要是用来构建support image和query image的关系，细节做法：

1. 所有的图片过embedding module得到对应的feature map，对于C shot的C > 1的话就把所有同类图片的feature map做一下element wise sum得到一个代表这个类别的feature map
2. 得到C个feature map之后依次和query image的feature map concat到一起送入到一个conv block里面做回归，la
3. bel的话就是如果query image属于某个类，那个位置就为1否则为0，让网络去学习图片与类别之间的关系

注意点：
1. 模型的训练还是去fine tune，先在训练集上构建episode训练然后再到support集上fine tune
2. 对于zero shot，一般会给出关于类别的语意信息，那么就把原先support set产生的feature map直接改成类别的semantic embedding就好，然后其他ppl就和K shot保持一致

下面这张图就是RN网络的细化：

![](Learning-to-Compare-Relation-Network-for-Few-Shot-Learning-屏幕快照 2020-04-14 下午6.12.30.png)


贴一下结果吧,放到现在在miniImageNet上的结果就不高了:

![](Learning-to-Compare-Relation-Network-for-Few-Shot-Learning-屏幕快照 2020-04-14 下午7.13.04.png)
