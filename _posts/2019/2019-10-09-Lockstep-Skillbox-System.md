﻿---
layout: post
title: "技能系统组件剖析"
date: 2019-10-09
categories: 源码解读
tags: Lockstep skill
excerpt: 对技能系统组件的解析，并通过技能系统反推自定义组件的创建
mathjax: true
---

* content
{:toc}

![](https://longshilin.com/images/20191009151525.png)
在上面这张图中，展示的是技能系统组件的继承关系，最顶层IComponent接口是继承的自定义生命周期接口ILifeCycle。通过这个CSkillBox类，管理整个的技能系统。

首先它是一个组件类，并实现ISkillEventHandler事件接口，可以通过实现该事件接口的方法，然后通过回调的方式来管理技能释放的事件。

接下里着重讲讲技能系统是如何调度和管理技能释放的。
