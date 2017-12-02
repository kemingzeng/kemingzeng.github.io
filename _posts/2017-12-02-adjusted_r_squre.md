---
layout: post
title: Adjusted R-square
date: 2017-12-02
categories: blog
tags: [ML]
description: 评估回归质量，并且可用于特征筛选。
---

### 一点统计学的小知识
![](https://raw.githubusercontent.com/zkm670541684/zkm670541684.github.io/master/assets/image/ars_1.png )

### 所以什么是R-square
\\[R^{2} = 1-\frac{RSS}{TSS}\\]
* ESS：回归平方和（可解释平方和）
* RSS：残差平方和
* TSS：总平方和
\\[TSS=ESS+RSS\\]

### 代表什么含义
* ESS：模型可线性解释的部分占TSS的比例
* RSS：模型不可线性解释的部分占TSS的比例
* TSS：可以理解为方差，代表真实数据的波动
这种理解基本上是玄学。

### 数学公式
\\[\sum_{i=1}^{n}(y_{i}-\bar{y})^{2}=\sum_{i=1}^{n}(\tilde{y_{i}}-\bar{y})^{2}+\sum_{i=1}^{n}(y_{i}-\tilde{y_{i}})^{2}\\]

### 为什么用adjusted
* 对特征维数的惩罚，毕竟有奥勒姆剃刀原理。
