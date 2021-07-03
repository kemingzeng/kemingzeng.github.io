---
layout: post
title: 对设计模式的理解
date: 2021-07-03
categories: blog
tags: [memory]
description: 几种设计模式。
---

##### 设计模式

- 这玩意，自己不写，永远都不会有感觉，列几种常见的吧。
- 继承
  - 算不上设计模式
  - 把共性往父类提
- 策略模式
  - 把特性归到一起，单独拿出来，作为成员，插装到类中
- 桥接模式
  - 如果有两个维度的变化，第一个是形状，第二个是颜色，互相独立，可以一个使用继承，一个使用策略。
  - 整体来看，就是一种桥接模式
  - [here](https://www.runoob.com/design-pattern/bridge-pattern.html)
- 组合模式
  - ProjectManager对象的成员有`vector<Employee*>`，employee作为父类或者是接口存在。
  - 这样在实现方法的时候就可以自己写自己的，一层管一层。
  - [here](https://www.runoob.com/design-pattern/composite-pattern.html)
- 访问者模式
  - 对于一棵有复杂层级关系的树或图（其中节点有多种类型），想要有序打印各个节点的信息应该怎么做呢？
  - 对于c++，通常会想到在每个节点class上override `friend operater<<`。
  - 实际上，打印功能不应该附加到本已经非常复杂的节点中，它可以独立存在。
  - 对于访问者模式，打印功能实现在访问者上面，层级关系的关系(接待)实现在被访问者（节点）上面。
  - [here](https://www.runoob.com/design-pattern/visitor-pattern.html)
- 适配器模式
  - 用新接口适配老接口
  - 继承或依赖都可以做到，最好还是选用依赖，把老接口作为类的一个成员，新接口使用时实际调用老接口就好了
- 观察者模式
  - 用的最多的就是它，也没啥好说的
- 单例模式
  - 一般用 `Mayer's Singleton` 保证保证线程安全
