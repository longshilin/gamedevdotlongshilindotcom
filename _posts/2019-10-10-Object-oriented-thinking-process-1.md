---
layout: post
title: "《面向对象的思考过程》读书笔记一：面向对象中的四要素封装、继承、多态和组合"
date: 2019-10-10
categories: 读书笔记
tags: 面向对象
excerpt: 这是我关于阅读《面向对象的思考过程》的读书笔记（第一篇），记录最基础但是需要掌握的面向对象常识。
mathjax: true
---

* content
{:toc}

# 序言
在这里会记录面向对象中四个要素的一些知识点，面向对象的四要素是：
- 封装
- 继承
- 多态
- 组合

往往只有具备这四要素，才能称之为通过面向对象的思想在编程。下面我们对这四要素逐个进行说明。
# 封装和数据隐藏
使用对象的一个显著好处是对象无需暴露它的所有属性和行为。在出色的面向对象设计（至少通常认为是好的设计）中，对象仅暴露必要的接口来和其他对象进行交互。除了如何使用该对象，其他细节都应当对其他对象隐藏起来。

封装是基于对象既包含属性也包含行为这一事实。数据隐藏是封装的主要部分。

例如，一个计算数字的平方值的对象必须提供一个接口来展示结果。然而，调用方无需知道该对象计算平方值的内部属性和算法。健壮的类会一直拥有良好的封装。下一小节中，我们会讲解接口和实现的概念，这是封装的本质。

# 继承
面向对象程序设计的最强大的功能之一就是代码重用。结构化设计提供的代码重用非常受限。你可以编写一个功能块，然后多次重用它。但是面向对象的设计更进一步，允许你定义类之间的关系，通过组织和识别不同类之间的共性，不仅可以实现代码重用，也可以指导设计。继承是实现该功能的主要手段。

## is-a关系
在Shape（形状）例子中，Circle（圆形）、Square（矩形）和Star（星形）都直接继承自Shape。这种关系通常被称为is-a关系，因为圆是一个形状，而矩形也是形状。当子类继承自父类时，任何父类能做的事情子类都可以做。即Circle、Square和Star都是Shape的扩展。

>补充：C#中abstract和virtual的区别？
>
>虚方法具有一个实现，并为派生类提供重写它的选项。
 抽象方法不提供实现，而是强制派生类重写该方法。
 因此，抽象方法中没有实际的代码，并且子类必须重写该方法。
 虚方法可以具有代码，这通常是某些东西的默认实现，并且任何子类都可以使用override修饰符覆盖该方法并提供自定义实现。[参考](https://stackoverflow.com/a/14729005/7739839)

# 多态
多态是一个希腊词，字面上理解为许多形状。尽管多态与继承是紧耦合的关系，但它通常单独作为面向对象技术中最强大的优点之一。当向一个对象发送一个消息时，该对象必须定义一个方法来响应这个消息。在继承体系图中，所有的子类从它们的超类中继承接口。然而，由于每个子类是单独的实体，每个子类需要对同一个消息有单独的应答。下面通过一个例子来说明多态的作用。

[多态代码片段](https://github.com/longshilin/Object-orientedThkingProcess/tree/master/Object-orientedFourElements/%E5%A4%9A%E6%80%81)
这里定义一个基类Shap，以及两个子类Circle和Rectangle。超类中定义面积属性和计算面积的抽象方法，子类中各自有具体的实现。代码片段贴出来如下：
```c#
namespace Object_orientedFourElements.多态
{
    public abstract class Shape
    {
        protected double area;

        public abstract double getArea();
    }
}
```
```c#
using System;

namespace Object_orientedFourElements.多态
{
    public class Circle : Shape
    {
        private readonly double radius;

        public Circle(double radius)
        {
            this.radius = radius;
        }

        public override double getArea()
        {
            area = Math.PI * Math.Sqrt(radius);
            return area;
        }
    }
}
```

```c#
namespace Object_orientedFourElements.多态
{
    public class Rectangle : Shape
    {
        private readonly double length;
        private readonly double width;

        public Rectangle(double length, double width)
        {
            this.length = length;
            this.width = width;
        }

        public override double getArea()
        {
            area = length * width;
            return area;
        }
    }
}
```

**最后调用Main方法中实例化子类对象，然后通过声明父类并调用父类的抽象方法，使用多态意味着给抽象类发送同样的`shape.getArea()`方法，实际上是调用对应子类的实现方法，以此完成多态的调用。**
```c#
using System;
using System.Collections;

namespace Object_orientedFourElements.多态
{
    class SMain
    {
        static void Main(string[] args)
        {
            Stack stack = new Stack();
            var circle = new Circle(5);
            var rectangle = new Rectangle(4, 5);
            stack.Push(circle);
            stack.Push(rectangle);

            while (stack.Count != 0)
            {
                Shape shape = (Shape) stack.Pop();
                Console.WriteLine($"Area = " + shape.getArea());
            }
        }
    }
}
```

# 组合
对象中包含其他对象非常自然。比如电视机包含开关和显示屏。计算机包含显卡、键盘和光驱。可以将计算机自身看成一个对象，而光驱也是一个有效的对象。

事实上，你可以打开计算机把光驱取下来放到你手上。计算机和光驱都可以看作对象。只是计算机包含其他的对象（譬如光驱）。

使用其他对象来构建或结合成新的对象，这种方式就是组合。

和继承一样，组合也是一种构建对象的机制。事实上，我想说只有两种方式来使用其他类构建新类，这两种方式就是继承和组合。

## has-a关系
我们之前已经讨论过了继承关系是is-a关系的原因，而组合关系可以称为has-a关系。我们可以使用上一小节中的例子，电视有（has-a）开关和显示屏。电视显而易见不是一个开关，所以两者没有继承关系。同样，计算机有（has-a）显卡，有（has-a）键盘，有（has-a）光驱。


# 结语
在面向对象的四要素中，经过上面的讨论之后可以有一个更深入的理解：
- 封装。把数据和行为封装到单个对象中是面向对象开发中的重中之重。单个对象既包含自身的数据，也包含自身的行为，并且可以向其他对象隐藏自身的某些东西。
- 继承。类可以继承自另一个类，并且可以使用父类中定义的属性和方法。
- 多态。多态意味着相似的对象对相同的消息有着不同的响应。例如，你可能拥有一个有很多形状的系统。然而圆、正方形和星形的绘制方式不同。使用多态你可以给这些形状发送相同的消息（例如Draw方法），每个形状可以响应自身的绘制。
- 组合。组合意味着可以使用其他对象来构建新对象。

下一篇我们继续以面向对象的方式进行思考。