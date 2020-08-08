﻿﻿﻿一个框架要和Unity本身的调用机制结合在一起，这个框架才有灵魂，也才真正跑起来，下面我会根据自己的见解剖析该框架如何实现这个结合的。

GameFramework框架本身分为两部分，一部分是纯逻辑层 GameFramework.dll，这是一个dll文件，查看内部代码的时候需要反编译，这部分代码可以脱离Unity引擎本身而独立存在的，这也是底层最核心的部分，因为逻辑代码终归是逻辑代码，不限制于某个特定平台的框架才是好的通用型框架。第二部分是和Unity引擎绑定的部分，需要借助Unity的生命周期来使得框架能更好的在Unity引擎中运行，这部分因为代码直接给出来来，所以我们接触的也更多。

还是从Unity点击Play按钮开始，我们最想进入的是Unity的Awake方法，这也是Unity游戏生命周期中最开始调用的方法。
```c#
using UnityEngine;

namespace UnityGameFramework.Runtime
{
    /// <summary>
    /// 游戏框架组件抽象类。
    /// </summary>
    public abstract class GameFrameworkComponent : MonoBehaviour
    {
        /// <summary>
        /// 游戏框架组件初始化。
        /// </summary>
        protected virtual void Awake()
        {
            GameEntry.RegisterComponent(this);
        }
    }
}
```
很明显上面的GameFrameworkComponent是继承至MonoBehavior的，相当于与Unity有相同的生命周期，成功将Unity的生命周期引入到框架的组件抽象类中，其后所有继承至该抽象类的组件都拥有Unity的生命周期，各个组件脚本也就可以直接挂载在Unity的Component面板上，下面这个是继承至GameFrameworkComponent抽象类的所有组件，一次显示不全分两次截取。

![](https://longshilin.com/images/20191117110955.png)
![](https://longshilin.com/images/20191117111046.png)
也就是下面这些：
```text
<Assembly-CSharp>\Assets\GameMain\Scripts\BuiltinData\BuiltinDataComponent.cs
<Assembly-CSharp>\Assets\GameMain\Scripts\HPBar\HPBarComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Base\BaseComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Base\ReferencePoolComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Config\ConfigComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\DataNode\DataNodeComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\DataTable\DataTableComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Debugger\DebuggerComponent.SettingsWindow.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Download\DownloadComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Entity\EntityComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Event\EventComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Fsm\FsmComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Localization\LocalizationComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Network\NetworkComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\ObjectPool\ObjectPoolComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Procedure\ProcedureComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Resource\ResourceComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Scene\SceneComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Setting\SettingComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\Sound\SoundComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\UI\UIComponent.cs
<UnityGameFramework.Runtime>\Assets\GameFramework\Scripts\Runtime\WebRequest\WebRequestComponent.cs
```
其中20个是Runtime包里面的自带组件，如下图各个组件都挂载了相应的脚本；而另外两个自定义组件就是Customs节点下面的两个组件，也在下面这张图中。

![](https://longshilin.com/images/20191117103818.png)

这样就一目了然了，因此框架中的各个功能模块通过这些组件组合而成的，把这些组件维护好，整个框架也就能正常运作了。

接下来剖析最开始提到的注册各个组件的代码片段，`GameEntry.RegisterComponent(this);` 这里this指的就是GameFrameworkComponent抽象类，这个RegisterComponent方法旨在将所有继承至该抽象类的方法，都统一注册到一个链表中存储以便后续统一维护。这个链表类型是`GameFrameworkLinkedList<GameFrameworkComponent>`，而对所有组件进行统一调度的类叫做`UnityGameFramework.Runtime\Assets\GameFramework\Scripts\Runtime\Base\GameEntry.cs`。这个类提供组件的register和get，以及游戏的quit和restart等。





