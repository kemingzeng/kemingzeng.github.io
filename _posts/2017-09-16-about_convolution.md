---
layout: post
title: About Convolution
date: 2017-09-16
categories: blog
tags: [ML]
description: 现在常用的卷积
---

### 卷积神经网络
- 这个没什么好说的，凡是做深度学习的人没有不了解二维卷积的，这几天看Xception，突然发现自己了解的卷积还停留在Lenet时代，实在不应该，所有稍微谈一谈。

### Ｆactorized Convolution
- 之前是看过这样一篇论文的，大家有兴趣可以去看看，还有一篇[相关的博客](http://blog.csdn.net/shenxiaolu1984/article/details/52266391)写的不错。
- 简单说一下，这篇论文的思路是通过分离卷积层的方式来优化卷积，使其计算效率提高，具体有这么两个套路：
	1. 中间加入base层，其卷积核可适当小一点，减少计算量，后面再通过１*１的卷积到我们想要的通道数。这个的变种是它所谓的stack方式，base层的卷积核数目为１．
	2. 每个卷积核干脆就局部感知，只管最近的几个通道。
-　这样搞效果还是不错的。

### Xception
- 17年初有个比较出名的网络，叫Xception，这个东西其实就是对Google Inception模块的极致化，[这个博客](http://www.mamicode.com/info-detail-1724092.html)画的图还不错～
- Xception模块和Inception的两大区别在于：
	1. 前者只在最后加了非线性变换，中间过程都是线性的，而后者全是relu.
	2. 前者是先channel-wise卷积，再1*1卷积，而后者先1*1卷积。

### Separable_conv和Depthwise_conv
- 因为要用keras，所以就想知道这两是干嘛的，到底对应的上面哪种？
- keras代码写的不清不楚的，只能往底层看tensorflow，惊奇地发现sep是两层，而dep是一层。
- dep可以看成是base层作用完的结果，sep的第一层是dep，第二层是base层之后1*1的部分（不一定是1*1）。
- 这两个函数都没有涉及到非线性变换，所以是可以再定义的。

