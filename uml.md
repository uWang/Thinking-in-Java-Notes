# UML：泛化、实现、依赖、关联、聚合、组合

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

