---
layout: post
title: "Unity Tips"
date: 2020-04-01
categories: UnityTips
tags: 
excerpt: 收集一些Unity Tips
mathjax: true
---

* content
{:toc}

# Tips-1 下载的资源文件的位置

很少需要直接访问从 Asset Store 下载的文件。但是，如果确实需要，可以在以下路径中找到这些文件：
```text
macOS：~/Library/Unity/Asset Store
Windows：C:\Users\accountName\AppData\Roaming\Unity\Asset Store
```
这些文件夹包含与特定 Asset Store 供应商对应的子文件夹。实际的资源文件以资源包发布者定义的结构包含在子文件夹中。

# Tips-2 Unity辅助图标控制柄位置开关

辅助图标控制柄位置开关: 可定义变换组件工具辅助图标的位置以及用于操纵辅助图标本身的控制柄。

辅助图标显示开关
![](https://longshilin.com/images/20200404110227.png)

![](https://longshilin.com/images/20200404105939.png)

**位置**
单击左侧的 Pivot/Center 按钮可在 Pivot 和 Center 之间切换。

Pivot 将辅助图标定位在网格的实际轴心点。
Center 将辅助图标定位在游戏对象渲染边界的中心。

**旋转**
单击右侧的 Local/Global 按钮可在 Local 和 Global 之间切换。

Local 保持辅助图标相对于游戏对象的旋转。
Global 将辅助图标固定在世界空间方向。

### Tips-3 Unity可交互对话框
图片演示

![](https://longshilin.com/images/20191117092517.png)

代码片段
```c#
#if UNITY_EDITOR
            if (Application.unityVersion != "2018.2.20f1" || Application.platform != RuntimePlatform.WindowsEditor)
            {
                UnityEditor.EditorUtility.DisplayDialog("警告", "此热更新 Demo 使用的资源包仅适用于 Unity 2018.2.20f1、Windows 系统平台版本，您当前使用的 Unity 版本或系统平台不匹配，这可能导致材质丢失等显示错误。", "知道了");
            }
#endif
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NDE4ODIzNzQsLTE5NTQ2NDc4OTYsMT
YyODU5MDkwNl19
-->