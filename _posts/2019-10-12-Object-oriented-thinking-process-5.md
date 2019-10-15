﻿---
layout: post
title: "《面向对象的思考过程》读书笔记五：创建对象及面向对象设计"
date: 2019-10-14
categories: 读书笔记
tags: 面向对象
excerpt: 这是我关于阅读《面向对象的思考过程》的读书笔记（第五篇），记载对象及面向对象设计相关知识。
mathjax: true
---

* content
{:toc}

任何组合类型都是has-a关系。然而，联合和聚合的微小区别在于部分如何构成整体。在聚合中，通常只看到整体，而在联合中，通常看到的是组成整体的部分。换句话说，聚合表示的是整体和部分的关系，意味着一个类由其他类组成，而联合则是两个类间提供服务。

联合和聚合关系区分:

![](https://longshilin.com/images/20191014180854.png)
- Dog类和Head类之间是聚合关系，因为头实际上是狗的一部分。连接这两个类图的线上的基数表示了一只狗只能有一个头。
- Dog类和Owner类之间是联合的关系。主人当然不是狗的一部分，反过来也一样，所以肯定不是聚合关系。然而狗需要主人的服务（遛狗）。


我们探索了组合关系，以及组合的两种主要形式：聚合和联合。继承表示一种新的已知对象，而组合表示各种各样的对象之间的交互关系。聚合和联合的示例及UML画图格式：
- 聚合：聚合由一个头部有一个菱形的线表示。汽车例子中，为了表示方向盘属于车的一部分

![](https://longshilin.com/images/20191014183829.png)

又比如，计算机自身代表了一种聚合，因为它是由主板、RAM等组成
![](https://longshilin.com/images/20191014184010.png)

- 联合：客户端/服务器端关系是这种模型。显而易见，客户端不是服务器端的一部分，而且服务器端也不是客户端的一部分，它们相互依赖。大部分情况下是服务器端为客户端提供服务。在UML标记中，一条单纯的线表示这种关系，线的两端没有任何形状

![](https://longshilin.com/images/20191014183710.png)