---
layout: post
title: "《面向对象的思考过程》读书笔记六：客户端和服务器交互 | 对象序列化传输流及反序列化"
date: 2019-10-15
categories: 读书笔记
tags: 面向对象
excerpt: 这是我关于阅读《面向对象的思考过程》的读书笔记（第六篇），记录一个实际服务器和客户端网络连接的例子，并且涉及有对象序列化传输和反序列化的相关知识点。
mathjax: true
---

* content
{:toc}

例子的大体思路是这样的，首先定义一个对象类，用于在网络中传输，并注入XML相关的标签，用于对象的序列化。接着编写客户端逻辑，实例化对象数据，并序列化到网络传输流中，传输到服务器，然后服务器反序列化该类，然后读取出类的相关信息。至此，整个对象在网络流中的传输成功完成！

在网络中进行传输的XML对象类
```c#
using System;
using System.Xml;
using System.Xml.Serialization;

namespace Object_orientedFourElements.客户端和服务器
{
    [XmlRoot("account")]
    public class CheckingAccount
    {
        private string _Name;
        private int _AccountNumber;

        [XmlElement("name")]
        public string Name
        {
            get => _Name;
            set => _Name = value;
        }

        [XmlElement("account_num")]
        public int AccountNumber
        {
            get => _AccountNumber;
            set => _AccountNumber = value;
        }

        public CheckingAccount()
        {
            _Name = "John Doe";
            _AccountNumber = 54321;
            Console.WriteLine("Creating Checking  Account!");
        }
    }
}
```

客户端类，主要发起socket请求并建立连接，对实例对象进行序列化包装和发送流，具体看代码注释。
```c#
using System;
using System.Net.Sockets;
using System.Xml.Serialization;

namespace Object_orientedFourElements.客户端和服务器
{
    public class Client
    {
        public static void Connect()
        {
            var checkingAccount = new CheckingAccount();
            try
            {
                // 创建TCP套接字
                TcpClient client = new TcpClient("127.0.0.1", 11111);
                // 准备将CheckingAccount对象序列化到XML中
                var xmlSerializer = new XmlSerializer(typeof(CheckingAccount));
                // 创建TCP流
                var networkStream = client.GetStream();
                // 序列化对象到TCP流
                xmlSerializer.Serialize(networkStream, checkingAccount);
                // 关闭所有资源
                networkStream.Close();
                client.Close();
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
                throw;
            }

            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }
    }
}
```

启动客户端的程序
```c#
using System;
using Object_orientedFourElements.客户端和服务器;

namespace test
{
    class Program
    {
        static void Main(string[] args)
        {
            Client.Connect();
        }
    }
}
```

服务器类，主要是接受客户端的信息流，并将其反序列化为实例对象，并打印出该类信息。
```c#
using System;
using System.IO;
using System.Net;
using System.Net.Sockets;
using System.Xml.Serialization;

namespace Object_orientedFourElements.客户端和服务器
{
    public class Server
    {
        public Server()
        {
            TcpListener server = null;
            TcpClient client = null;
            try
            {
                server = new TcpListener(IPAddress.Parse("127.0.0.1"), 11111);
                server.Start();

                // 设置输入缓冲区
                var bytes = new Byte[256];

                // 无限循环
                while (true)
                {
                    // 以阻塞模式获取到来的请求
                    client = server.AcceptTcpClient();
                    Console.WriteLine("Connected!");

                    // 打开流
                    var networkStream = client.GetStream();
                    // 从流中获取数据
                    int i;
                    while ((i = networkStream.Read(bytes, 0, bytes.Length)) != 0)
                    {
                        // 准备一个支持序列化器的流
                        var memoryStream = new MemoryStream(bytes);
                        // 准备序列化器
                        var xmlSerializer = new XmlSerializer(typeof(CheckingAccount));
                        // 从流中创建CheckingAccount对象
                        var account = (CheckingAccount) xmlSerializer.Deserialize(memoryStream);
                        // 现在演示确实创建了对象
                        Console.WriteLine($"Name: {account.Name}, Account Number: {account.AccountNumber}.");
                        // 抛出异常从而退出循环
                        throw new Exception("ignore");
                    }
                }
            }
            catch (Exception e)
            {
                if (!e.Message.Equals("ignore"))
                    Console.WriteLine(e);
            }
            finally
            {
                // 关闭资源
                client?.Close();
                server?.Stop();
            }

            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }
    }
}
```

服务器类的启动类
```c#
namespace Object_orientedFourElements.客户端和服务器
{
    public class SMain
    {
        static void Main(string[] args)
        {
            var server = new Server();
        }
    }
}
```

分两个项目，客户端独立一个项目，服务器独立一个项目，分别执行Main启动方法，整个Demo就可以跑起来了。跑起来的截图如下：

- 客户端控制台

![](https://longshilin.com/images/20191015152032.png)

- 服务器控制台

![](https://longshilin.com/images/20191015151948.png)

