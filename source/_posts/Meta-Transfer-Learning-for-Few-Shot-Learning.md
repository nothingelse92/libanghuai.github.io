---
title: Meta-Transfer Learning for Few-Shot Learning
date: 2020-04-16 10:53:15
tags:
  - Few Shot
  - Meta Learning
  - Classification
---
URL: https://arxiv.org/pdf/1812.02391.pdf

MAML一族的解决few shot learning的问题，对MAML的基本做法也做了比较详细的了解，论文所提的方法有两大主要内容:
1. Meta-Transfer Learning (MTL), 方法主要的Pipeline
2. Hard Task (HT) Meta-Batch, meta learning形式下的hard mining，做法比较简单粗暴

![](Meta-Transfer-Learning-for-Few-Shot-Learning-屏幕快照 2020-04-16 下午3.08.46.png)

整个Pipeline:
1. 首先需要明确整个pipeline里面涉及到的模型: **feature extractor(图片的特征提取),base learner(一个标注的分类模型), SS(一个用于学习kernel的scale和bias的shift的小子网络)**
2. 先用base class的所有数据训练一个feature extractor，这算是常规的操作, **这个feature extractor在后续所有的操作中都会被fix住**
3. Scaling and Shifting (SS), 这是Meta Transfer的核心，在每一轮meta task上它会和base learner同步一起学习，下图可以很好的解释SS的操作，为了避免过拟合，SS选择在feature extractor的基础上额外加一个小网络来学习transfer能力，而保证原来的feature extractor fix住，Scale学习的是kernel的scale，而Shift学习的是bias的shift，另外一种对现有网络的学习方式，下图也给了和一般的全面fine tune方式的比较：

    ![](Meta-Transfer-Learning-for-Few-Shot-Learning-屏幕快照 2020-04-16 下午3.21.15.png)

    当前这种考虑我的第一反应是SKNet之类对Kernel进行attention的方法感觉有点类似

4. HT Meta Batch: hide mining, 在每个meta test任务上选择acc最低的类别，然后把这些数据或者这些数据对应的类别拿出来再重点refine
5. 最后在meta test阶段fix住SS只在noval class上fine tune base learner.

伪代码:

![](Meta-Transfer-Learning-for-Few-Shot-Learning-屏幕快照 2020-04-16 下午3.27.24.png)

![](Meta-Transfer-Learning-for-Few-Shot-Learning-屏幕快照 2020-04-16 下午3.27.53.png)

贴一下点, 放到现在点就不算很高了:

![](Meta-Transfer-Learning-for-Few-Shot-Learning-屏幕快照 2020-04-16 下午3.29.39.png)
