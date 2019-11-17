---
title: High-Speed Tracking-by-Detection Without Using Image Information
date: 2019-01-31 13:17:55
tags:
  - Tracking
---
URL: http://ieeexplore.ieee.org/document/8078516/
一篇关于IoU tracker的论文，整个跟踪的逻辑都基于检测框来做，对于当前的帧f，检测模型首先会检测出这一帧上面所有的bounding box，然后利用贪心的逻辑将捡出来的框尝试加入到对应的track中，匹配的逻辑就是根据当前的bbox和track的bbox之间的IoU，然后IoU > IoU<sub>threshold</sub>,就把它加到当前track中否则看整个bbox的score，如果score  >= score<sub>threshold</sub> 并且 track的长度  >= length<sub>threshold</sub>就把当前帧作为这个track的结束帧，否则就直接终端当前的这个track，因为他有很大的概率是一段fp，当然论文中也提到匹配的时候也是可以用最大匹配（IoU最大）的一些算法来做的：

![](High-Speed-Tracking-by-Detection-Without-Using-Image-Information-image.jpg)
![](High-Speed-Tracking-by-Detection-Without-Using-Image-Information-屏幕快照 2019-01-29 下午8.01.57.png)
