﻿﻿﻿![](https://longshilin.com/images/20191107095720.png)

导航系统允许您创建可以导航游戏世界的角色。它使您的角色能够理解他们需要走楼梯才能到达二楼，或者跳过去越过沟渠。Unity NavMesh
系统，它由以下部分组成：

- **NavMesh**（导航网格的缩写）是一种数据结构，用于描述游戏世界的可行走表面，并允许查找游戏世界中从一个可行走位置到另一个位置的路径。数据结构是根据您的关卡几何自动构建或烘焙的。
- **NavMesh Agent**组件可帮助您创建在朝目标前进时彼此避开的角色。代理商使用NavMesh来推理游戏世界，他们知道如何避免彼此以及移动障碍物。
- **离网链接**组件允许您合并导航快捷方式，这些快捷方式无法使用可行走的表面来表示。例如，跳过沟渠或栅栏，或在穿过前打开一扇门，都可以描述为非网格链接。
- **NavMesh障碍物**组件使您能够描述代理商在导航世界时应避免的移动障碍物。由物理系统控制的桶或板条箱是障碍的一个很好的例子。当障碍物正在移动时，特工会尽力避免它，但是一旦障碍物静止，它将在导航网格上刻一个洞，以便特工可以改变其路径以绕过它，或者如果静止的障碍物正在阻碍路径方式，代理商可以找到不同的路线。

参考：https://docs.unity3d.com/Manual/nav-NavigationSystem.html


NavMesh可使用四个高级组件：

- NavMesh曲面 -用于为一种类型的代理构建和启用NavMesh曲面。

- NavMesh修改器 -用于基于变换层次结构影响NavMesh区域类型的NavMesh生成。™¡¡

- NavMeshModifierVolume-用于影响基于体积的NavMesh区域类型的NavMesh生成。

- NavMeshLink-用于为一种类型的代理连接相同或不同的NavMesh曲面。

另请参阅有关Mesh-BuildingComponents-API的文档。

有关代理类型的更多信息，请参阅有关创建NavMesh代理的文档。

有关NavMesh区域类型的更多详细信息，请参见有关NavMesh区域的文档。

参考：https://docs.unity3d.com/Manual/NavMesh-BuildingComponents.html

一篇官方文章教会你入门Navemesh
https://docs.unity3d.com/Manual/nav-CreateNavMeshAgent.html

至此，Navmesh的基本入门就完成了。

----

下面是Unity Simple Way System插件的简单入门，也是Demo项目中需要用到的一个插件，后面会使用这个插件完成角色顶点移动到下一个战斗区域的自动寻路功能。

> 可以下载插件中的[官方文档 SimpleWaypointSystem.pdf](https://longshilin.com/files/SimpleWaypointSystem.pdf)


下面我会将官方文档中提及的内容做一个简单描述。
刚想了下，还是直接贴我翻译之后的文档吧，这样更直观。[详见SimpleWaypointSystem.docx](https://longshilin.com/files/SimpleWaypointSystem.docx)
