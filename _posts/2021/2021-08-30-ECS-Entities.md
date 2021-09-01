---
layout: post 
title: "ECS Entities"
date: 2021-08-31 
categories: DOTS 
tags: unity ecs dots 
excerpt: Entities官方文档译文（Entities 0.17.0-preview.42）
---
* content 
{:toc}

# Entities

Entities是 `Entity Component System` 体系结构的三个主要元素之一。它们代表了游戏或应用程序中的个体“事物”。一个实体既没有行为也没有数据；相反，它标识了哪些数据片段属于一起。`Systems` 提供行为，`Components` 存储数据。

实体本质上是一个ID。最简单的方式来考虑它是一个轻量级的游戏对象，甚至没有一个默认的名称。实体ID是稳定的；你可以使用它们存储对另一个组件或实体的引用。例如，层次结构中的子实体可能需要引用其父实体。

[EntityManager](#) 管理World中的所有实体。EntityManager维护实体列表并组织与实体关联的数据，已获得最佳性能。

虽然实体没有类型，但是可以根据与其关联的数据组件的类型对实体组进行分类。当你创建实体并向其添加组件时，EntityManager会跟踪现有实体上组件的唯一组合。这种独特的组合被称为原型。EntityManager在向实体添加组件时创建 [EntityArchetype](#) 结构。你可以使用现有的 `EntityArchetypes` 来创建符合该原型的新实体。你还可以提前创建一个 `EntityArchetype` 并使用它来创建实体。

## 创建实体

创建实体最简单的方法是使用Unity编辑器。你可以设置ECS在运行时将场景和Prefabs中的游戏对象转换为实体。对于游戏或应用程序的更多动态部分，可以创建在job中创建多个实体的 `Spawning Systems`。最后，你可以使用 [EntityManager.CreateEntity](#) 函数来一次性创建多个实体。使用EntityManager创建实体通常是效率最低的方法。

## 使用EntityManager创建实体

使用 [EntityManager.CreateEntity](#) 函数中的一个来创建实体，ECS在于EntityManager相同的World中创建实体。

你可以通过以下方式逐个创建实体：
- 

<!--
https://interpreter.caiyunai.com/html/612e3c17e03ec236995fcbd2
-->
















