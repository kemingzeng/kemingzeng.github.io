---
layout: post
title: Sigmoid Loss
date: 2017-08-22
categories: blog
tags: [ML]
description: 关于sigmoid与softmax的loss
---

### sigmoid与softmax
- 这两个东西拿到一起比较基本就是它们在最后一层做分类了。
- 对于二分类，我们当然会首先想到sigmoid，大名鼎鼎的logistic回归嘛！一旦类别超过2，我们也很快会想到softmax。事实上，对于softmax来说，二分类同样可以做，无非是个二元的one-hot标签。
- 从效果来说，两者相差不大，因为原理上都是相似的，softmax甚至可以看做是sigmoid的推广。

### sigmoid最适配的loss
- 很久以前还有人用均方误差的，不过现在都改成了交叉熵。原因很简单，交叉熵更适合梯度下降！不过在深度的网络中，sigmoid也用的很少了。
- 至于原因，参照这个推导，从求导的角度阐述了这个问题
[交叉熵代价函数](http://blog.csdn.net/u014313009/article/details/51043064) 

### 至于softmax
- 因为交叉熵是针对离散分布的，之前我一直没太明白对于one-hot它怎么计算，还傻傻的以为类别取0,1,2，……，然后一个个乘以预测概率的对数再加和。
- 这个想法有点问题，直接上代码比较好。
[tensorflow（尊重知乎劳动成果）](https://zhuanlan.zhihu.com/p/27842203)