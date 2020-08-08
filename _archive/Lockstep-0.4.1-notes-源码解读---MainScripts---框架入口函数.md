说到主入口函数，那么肯定是已组件的形式挂载在场景中的，然后通过Unity的生命周期来驱动这个框架的入口函数，进入驱动整个框架进行运转。这是自定义框架和Unity生命周期进行打交道的位置，所以值得拿来说一说。

我们的主要目的是首先分析清除作者写这个框架时，各个代码模块的作用，然后尝试去整理归纳可以学习借鉴的方面，最后分析这些知识点背后存在的原理。

言归正传，MainScripts一共抽取了Unity的五种生命周期阶段进行重写，下面对MainScript在这五个生命周期中做的事情做一个剖析。



 ##### Awake

挂载两个组件PingMono和InputMono，有关这两个自定义组件的相关信息，在后面会专门开一篇文章来讲述。

然后实例化UnityServiceContainer，这个是Unity平台才会去创建的，对于纯dotnet平台，如果要去跑框架的逻辑层的话，那么是创建PureServiceContainer。那么具体这两个ServiceContainer的作用是什么呢，后面我也会开专门的专题来说明。这里大致提一下，就是框架中所有Service的总管理类，通过这个ServiceContainer可以获取到框架中任何一个正在使用的Service类型。

紧接着是通过这个总管理类，来设置一个模块Service的信息，比如设置ConstStateService中的GameName（游戏名称）、IsClientMode（是否是单机客户端模式）、IsVideoMode（是否是录像播放模式）以及MaxEnemyCount（最大的敌人数量）。

接下来是是Unity的日志打印API订阅框架中的日志打印委托函数，实现框架的日志系统输出到Unity平台上。这个解耦方式的好处是，如果框架是跑在别的平台比如UE，我只需要将别的平台的日志打印API也订阅上，就能实现跨平台。这个解耦方式值得学习，因为一个好的框架，能够支持跨平台也是超级棒的了！

最后是屏幕大小设置，然后调用launcher启动器中的DoAwake方法。我这篇文章只讲MainScripts这一层的东西，对于Launcher启动器，会另外开一篇去说明。

##### Start

通过上面Awake生命周期中实例化的总的Service总管理器获取ConstateService对象，然后设置日志打印输入的持久化文件路径，然后调用launcher中的DoStart方法。

##### Update

这里是每帧都会执行一次的Update方法，也是整个框架的驱动方法。更新ConstStateService中的IsRunVideo（是否勾选RunVideo）。另外就是调用launcher中的DoUpdate方法，在DoUpdate方法中传入的参数是每一帧所花费的时间`Time.deltaTime`。

##### OnDestroy

调用launcher中的DoDestroy方法，所有的销毁工作交由launcher启动器的对应方法去做。

##### OnApplicationQuit

调用launcher中的OnApplicationQuit方法，它会帮我们做好应退出时需要做的工作。



贯穿下来整个UnityEngine的生命走起调用完成时，整个游戏也就执行完毕，这里有游戏的初始化，开始准备，以及每帧调用，最后游戏内容销毁和应用退出的处理过程。从这个宏观的角度来看待一个游戏的生命周期，就是这么点东西，而如何协调好各个Service模块之间的工作，是微观角度来看待的事情，也是值得去深究的。

下一篇我们将从一个稍微细分的角度来继续剖析整个生命周期，那就是launcher启动器类的运作细节。

Let's Go~

