---
layout: post 
title: "ECS Entities"
date: 2021-08-31 
categories: DOTS 
tags: unity ecs dots 
excerpt: Entities官方文档译文（Entities 0.17.0-preview.42），[原文链接](https://docs.unity3d.com/Packages/com.unity.entities@0.17/manual/ecs_entities.html)
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
- 使用 ComponentType 数组创建组件实体
- 使用 [EntityArchetype](#) 创建组件实体
- 使用 [Instantiate](#) 拷贝一个现有的实体，包括其当前数据。
- 创建一个没有组件的实体，然后向其添加组件。（你可以立即添加组件，或者在需要其他组件时添加。）

你还可以一次创建多个实体：

- 使用 [CreateEntity](#) 将具有相同原型的新实体填充NativeArray。
- 使用 [Instantiate](#) 将现有实体的副本填充NativeArray，包括其当前数据。

## 添加和删除组件

创建实体后，您可以添加或删除组件。执行此操作时，受影响实体的原型会发生变化，EntityManager 必须将更改后的数据移动到新的内存块，并压缩原始块中的组件数组。

对导致结构变化的实体的更改——即添加或删除更改 `SharedComponentData` 值的组件，以及破坏实体——不能在作业内部完成，因为这些可能会使作业正在处理的数据无效。相反，你将这些类型的修改以命令的形式添加到 [EntityCommandBuffer](https://docs.unity3d.com/Packages/com.unity.entities@0.17/api/Unity.Entities.EntityCommandBuffer.html) 并在Job完成之后执行这个 command buffer.

EntityManager 提供了从单个实体以及 NativeArray 中的所有实体中删除组件的功能。有关更多信息，请参阅有关文档 [Components](https://docs.unity3d.com/Packages/com.unity.entities@0.17/manual/ecs_components.html).

## 迭代实体

迭代具有一组匹配组件的所有实体，是 ECS 架构的核心。参考 [Accessing entity data](https://docs.unity3d.com/Packages/com.unity.entities@0.17/manual/chunk_iteration.html).















