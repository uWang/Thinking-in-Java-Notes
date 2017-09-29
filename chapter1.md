# 第1章  对象导论

---
本章应该算是全书的一个总领，这个总领就不简单呐。藏了很多点。  

首先摘录下一开头对编程语言的概述：
> 计算机革命起源于机器，因此，编程语言的产生也始于对机器的模仿。
> 但是，计算机并非只是机器那么简单。计算机是头脑延伸的工具（就像Steve Jobs常喜欢说的“头脑的自行车”一样），同时还是一种不同类型的表达媒体。**因此，这种工具看起来已经越来越不像机器，而更像我们头脑的一部分**，以及一种如写作、绘画、雕刻、动画、电影等一样的表达形式。面向对象程序设计（Object-oriented Programming, OOP）便是这种以计算机作为表达媒体的大趋势中的组成部分。


### 1.3 每个对象都提供服务
>当正在试图开发或理解一个程序设计时，最好的方法之一就是将对象想像为“服务提供者”。程序本身将向用户提供服务，它将通过调用其他对象提供的服务来实现这一目的。**你的目标就是去创建（或者最好是在现有代码库中寻找）能够提供理想服务来解决问题的一系列对象。**

- **高内聚**是软件设计的基本质量要求之一。人们在设计对象时所面临的一个问题是，将过多的功能都塞在一个对象中。  
例如：在检查打印模式的模块中，你可以这样设计一个对象，让它了解所有的格式和打印技术。你可能发现，这些功能对于一个对象来说太多了。可以这样设计：
	- 一个对象可以是所有可能的支票排版的目录
	- 一个对象可以是一个通用的打印接口，它知道相关打印机的信息（不包括簿记的内容）
	- 一个对象通过调用另外两个对象的服务来完成打印任务。
这样每个对象都有一个他所能提供服务的内聚的集合。

### 1.4 被隐藏的具体实现

- Java里的访问控制

|  修饰符   | 当前类 | 同一包 | 子孙类 | 其他包 |
|-----------|--------|--------|--------|--------|
| private   | Y      | N      | N      | N      |
| default   | Y      | Y      | N      | N      |
| protected | Y      | Y      | Y      | N      |
| public    | Y      | Y      | Y      | Y      |

### 1.5 复用具体实现

- 感同身受
> 事实证明，这种复用性并不容易达到我们所希望的那种程度，产生一个可复用的对象设计需要丰富的经验和敏锐的洞察力。

### 补充UML：泛化、实现、依赖、关联、聚合、组合
> 参考1：[http://www.uml.org.cn/oobject/201104212.asp](http://www.uml.org.cn/oobject/201104212.asp)  
> 参考2：[刘水镜的CSDN博客](http://blog.csdn.net/liushuijinger/article/details/6994265)

- 泛化(generalization)
	- 表示is-a的关系，子类继承父类。
	- *实线三角箭头子类指向父类。*  
![generalization](http://7xkzms.com1.z0.glb.clouddn.com/Generalization.jpg)

- 实现 (Realization)
	- 接口和实现类的关系。
	- *虚线三角箭头实现类指向接口。*  
![Realization](http://7xkzms.com1.z0.glb.clouddn.com/Realization.jpg)

- 依赖 (Dependency)
	- 对象之间最弱的一种关联方式，是临时性的关联。
	- 代码中一般指由**局部变量、函数参数、返回值**建立的对于其他对象的调用关系。
	- *虚线箭头从使用类指向依赖的类。*  
![Dependency](http://7xkzms.com1.z0.glb.clouddn.com/Dependence.jpg)

- 关联 (Association)
	- 对象之间一种引用关系，比如客户类与订单类之间的关系。
	- 这种关系通常使用**类的属性**表达。
	- 关联又分为一般关联、聚合关联与组合关联。  
	- *使用带箭头的实线表示，箭头从使用类指向被关联的类。可以是单向和双向。*  
![Association](http://7xkzms.com1.z0.glb.clouddn.com/Association.jpg)


- 聚合 (Aggregation)  
	- 表示has-a的关系，是一种不稳定的包含关系。较强于一般关联,有整体与局部的关系,并且**没有了整体,局部也可单独存在**。
	- 如公司和员工的关系，公司包含员工，但如果公司倒闭，员工依然可以换公司。  
	- *使用空心的菱形表示，菱形从局部指向整体。*  
![Aggregation](http://7xkzms.com1.z0.glb.clouddn.com/Aggregation.gif)


- 组合 (Composition)  
	- 表示contains-a的关系，是一种强烈的包含关系。组合类负责被组合类的生命周期。是一种更强的聚合关系。**部分不能脱离整体存在**。
	- 如公司和部门的关系，没有了公司，部门也不能存在了；调查问卷中问题和选项的关系；订单和订单选项的关系。  
	- *使用实心的菱形表示，菱形从局部指向整体。*  
![Composition](http://7xkzms.com1.z0.glb.clouddn.com/Composition.gif)

- 组合与聚合
	+ 聚合关系的类里含有另一个类作为参数  
		雁群类（GooseGroup）的构造函数中要用到大雁（Goose）作为参数把值传进来 大雁类（Goose）可以脱离雁群类而独立存在 
	+ 组合关系的类里含有另一个类的实例化  
		大雁类（Goose）在实例化之前 一定要先实例化翅膀类（Wings） 两个类紧密耦合在一起 它们有相同的生命周期 翅膀类（Wings）不可以脱离大雁类（Goose）而独立存在

``` java
//雁群类：
public  class GooseGroup  
{  
    public Goose goose;  

    public GooseGroup(Goose goose)  
    {  
        this.goose = goose;  
    }  
}  
```
		
```java
//大雁类
public class Goose  
{  
    public Wings wings;  
  
    public Goose()  
    {  
        wings=new Wings();  
    }  
}  
```

