---
layout: post
title: Fit Curve Stability
date: 2017-12-19
categories: blog
tags: [ML]
description: 关于深度学习中fit的准确率曲线不稳定问题。
---

### 正常的fit曲线
* 正常情况下， 我们在拟合的时候，随着epoch的增加，我们的train acc和val acc会逐渐趋于稳定。
* 当然，这是在使用一些自适应步长方法的时候，如果只是SGD，可能训练着acc就飘了。

### 最近训练遇到的
* 图就是这样
![](https://raw.githubusercontent.com/zkm670541684/zkm670541684.github.io/master/assets/image/fcs_1.png )

### 理解
* 首先要明确的是，train acc的下降并不能说明模型出现了什么问题。这只是训练方法的问题。
* 可能参数空间在极小点周围并不平滑，有极大值的尖点出现。我们需要设计方法来规避这些极大值。
* 考虑的方法是保存这样一个较好值，再重新修改lr进行fine tune。

### 启示
* 因为这种问题遇见的不多，最开始我怀疑是数据或者模型的问题。
* 它给我们提了个醒，自适应的优化器并不是尽善尽美的，我们绝不能将其当作黑盒。
