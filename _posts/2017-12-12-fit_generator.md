---
layout: post
title: Adjusted R-square
date: 2017-12-12
categories: blog
tags: [ML]
description: 关于keras中fit generator用于多个数据文件。
---

### fit generator
* 这个东西是keras里面很好用的一个训练器
* 知道它之前，对于小数据，我只会用fit；数据量大一点，内存装不下了，只好分块存在硬盘上，训练时读取存好的npy文件，再分批次使用fit on batch。这么搞的话，每次都要自己手动分batch，比较麻烦，而且训练时要反复读取数据，增加了计算量。
* fit generator是个好东西，传给它一个x，y的generator就ok。

### 一些小细节
* generator可以自己写，也可以直接用Imagedatagenerator生成，推荐后者，flow一下，还可以把数据增强做了。
* Imagedatagenerator生成的generator是一个loop，也就是说它是可以一直next的，如果没有其他信息，我们不可能知道一个数据集里面滚多少batch就终止了。fit on generator有个重要参数，叫steps-per-epoch，就是用来控制这个的。

### 大数据量怎么办
* 数据量一大，内存就不可能装下了，generator就要来自多个数据文件，且不能同时读取，不然立刻OFM。
* 为了探寻解决方法，我做了很多尝试，终于找到了一种看起来不错的方式。

### 大数据下的解决方案
* 如此这般。
* fit generator那里的steps也要改。

···
from keras.preprocessing.image import ImageDataGenerator
import random
import numpy as np
from keras.models import Model
# 生成数据
a = []
b = []
for i in range(8):
    t = random.randrange(0, 255)
    a.append(t)
    t = random.randrange(0, 255)
    b.append(t)
a = np.array(a).reshape(2, 2, 2, 1)
b = np.array(b).reshape(2, 2, 2, 1)
y = np.ones([2, 2])

def train_gen(x1=a, x2=b, y=y):
    gen = ImageDataGenerator()
    while 1:
        x = x1     # 可以输入目录，在此读取
        gens = gen.flow(x, y, batch_size=1)     # 大量就写成循环
        for i in range(x1.shape[0]):
            yield gens.next()
        del x
        x = x2     # 清空1的内存后，再读2
        gens = gen.flow(x, y, batch_size=1)
        for i in range(x2.shape[0]):
            yield gens.next()

if __name__ == '__main__':
    print(a)
    print(b)
    print('---------------')
    g = train_gen()     # 这一步必须写在外面，很重要！
    for i in range(10):
        print(next(g))
···

![](https://raw.githubusercontent.com/zkm670541684/zkm670541684.github.io/master/assets/image/bg_1.PNG)
