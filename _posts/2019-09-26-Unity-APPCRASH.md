---
layout: post
title: "Unity Crash"
date: 2019-09-26
categories: Unity
tags: "unity crash"
excerpt: Unity APPCRASH!!! 
mathjax: true
---

* content
{:toc}

```c#
    /// <summary>
    /// 下一次技能增伤，增伤值用后就失效
    /// </summary>
    public FP OneTimeSkillAddHarm
    {
        get
        {
            FP temp = OneTimeSkillAddHarm;
            OneTimeSkillAddHarm = 0;
            return temp;
        }
        set { OneTimeSkillAddHarm = value; }
    }
```

上面这句代码，导致Unity崩溃！！！ （ps：一看就知道其中的bug）

乍一看自己把自己惊到辽，这是当时和另外一个字段交换名称时没有全部替换干净，是手动替换的，所以只替换了外面的声明，而里面的字段名称没有替换，才会导致这次的“事故”，称为事故一点也不稀奇。以后吸取教训，**`替换的时候一律用编辑器的快捷方式批量替换`**，避免这样的错误。

另外，也让我知道了这种方式能直接导致Unity崩溃，下面我贴出崩溃信息：
![](https://longshilin.com/images/20190926103939.png)

```text
问题签名:
  问题事件名称:	APPCRASH
  应用程序名:	Unity.exe
  应用程序版本:	2019.2.1.19802
  应用程序时间戳:	5d55558a
  故障模块名称:	ntdll.dll
  故障模块版本:	6.1.7601.24231
  故障模块时间戳:	5b6db5a7
  异常代码:	c0000005
  异常偏移:	000000000006b746
  OS 版本:	6.1.7601.2.1.0.256.1
  区域设置 ID:	2052
  其他信息 1:	3989
  其他信息 2:	39890c64589588970365dc6bc7dc0bf7
  其他信息 3:	2598
  其他信息 4:	25988acbd436253fe4088233002c9db6
```


今天算是干了一件大事 : )