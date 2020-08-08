﻿﻿﻿这一篇文章我们专门用来收集我在开发Demo时，总结的一些开发小技巧，以备今后使用~

### Unity可交互对话框
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


### Unity的Inspector界面自定义按钮，实现在Unity交互界面直接修改数据信息
代码片段
