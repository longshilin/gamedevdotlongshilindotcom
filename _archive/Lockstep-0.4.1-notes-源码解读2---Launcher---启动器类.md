Launcher启动器是比较核心的组件，因为它承担着将Unity的生命周期和框架的生命周期之间的调度桥梁的作用。可以说是框架内游戏生命周期最核心的管理类了。

首先启动器管理类拿到一些Service模块的变量，这些是管理器类中需要用到的。

💬 注意：获取值有两种类型，一种时通过方法的方式，一种是赋值的方式。这两者的区别在于方法获取可以拿到最新的值，而赋值方式拿的是固定不会修改的值。所以对于全局初始化的属性，直接通过赋值方式，对于要动态拿最新值，如目前运行帧数，则通过方法的方式。

```c#
// 方法获取最新的值
public int CurTick => _serviceContainer.GetService<ICommonStateService>().Tick;
public bool IsRunVideo => _constStateService.IsRunVideo;

// 取固定值
public int JumpToTick = 10;
```



##### DoAwake

实例化ManagerContianer，并将全局的serviceContianer容器中，实现ITimeMachine接口的类都注册到managerContianer中。另外实例化EventRegisterService和TimeMachineContianer，并注册到全局的serviceContainer服务管理容器内。

目前，全局_serviceContainer中注册的类共13个，包括：

```text
RandomService
CommonStateService
ConstStateService
SimulatorService
NetworkService
IdService
GameResourceService
GameStateService
GameConfigService
GameInputService
UnityGameViewService
TimeMachineContainer
EventRegisterService
```

ManagerContainer中包括的类：
```text
RandomService
ConstStateService
SimulatorService
NetworkService
GameResourceService
GameStateService
GameConfigService
UnityGameViewService
```

说到这里，我们来说说ServiceContainer中独有的RegisterService方法

```c#
public void RegisterService(IService service, bool overwriteExisting = true){
	var interfaceTypes = service.GetType().FindInterfaces((type, criteria) =>
			type.GetInterfaces().Any(t => t == typeof(IService)), service)
		.ToArray();

	foreach (var type in interfaceTypes) {
		if (!_allServices.ContainsKey(type))
			_allServices.Add(type, service);
		else if (overwriteExisting) {
			_allServices[type] = service;
		}
	}
}
```

① 我们假定调用这个方法的是下面这条语句：

```c#
RegisterService(new RandomService());
```
而且已知RandomService的UML关系图长下面这样：
![](https://longshilin.com/images/20191028150114.png)

在上面代码的第一段的意思是：在由当前RandomService所实现或继承的接口的筛选列表的对象数组（即IRandomService），筛选出由`IRandomService`所实现或继承的接口对象数组中类型为IService的接口（即IRandomService）。

第二段foreach的意思是：遍历上面找到的接口对象数组，然后根据接口类型和对应的接口实现或继承类Service，组成一个字典保存，如果支持覆盖，那么会覆盖更新key值相同的Service值。

② 我们假定调用的是一个更复杂的对象参数：
```c#
RegisterService(new TimeMachineContainer());
```
已知TimeMachineContainer的UML关系图如下：

![](https://longshilin.com/images/20191028161708.png)

接着分析代码片段中第一段中的意思是：在由当前`TimeMachineContainer`所实现或继承的接口的筛选列表的对象数组（即`ITimeMachineContainer` 和 `ITimeMachineService`），从中筛选出由`ITimeMachineContainer` 或 `ITimeMachineService`所实现或继承的接口对象数组中类型为IService的接口，以上两者都满足条件，即返回的接口对象数组是`ITimeMachineContainer` 和 `ITimeMachineService`。

