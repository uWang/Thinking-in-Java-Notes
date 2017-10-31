# 第7章 复用类

---
- 组合技术通常用于想在新类中使用现有类的功能而非它的接口这种情形。即，在新类中嵌入某个对象，让其实现所需的功能。
- 继承，使用现有类，并开发一个它的特殊版本。通常，这意味着你在使用一个通用类，并为了某种特殊需要而将其特殊化。

### 向上转型
- 向上转型是一个较专用类型向较通用类型转换（子类转成父类），所以总是很安全的。

### 再论组合和继承
- 到底是用组合还是继承，一个最清晰的判断办法就是：**问问自己是否需要从新类向基类进行向上转型**。如果必须向上转型，则继承是必要的，如果不需要，则应当好好考虑是否需要继承。

### final关键字
- final关键字根据上下文环境，可能有两种理由：设计和效率。
- 一个永远不改变的编译时常量。
- 一个在运行时被初始化的值，而你不希望它被改变。
- 对于基本数据类型，final使数值恒定不变，而对于对象引用，final使引用恒定不变。对象自身是可以被修改的。
- 空白final必须在域的定义处或者每个构造器中用表达式对final进行赋值。
- final类型入参一般用在匿名内部类的参数传递上。

### final方法
- final方法的意义1：无法被覆盖。
- final方法的意义2：效率。随着JVM的发展，不再需要使用final进行优化。
- 类中所有的private方法都隐式地指定为是final的。由于无法取用private方法，所以也就无法覆盖它。可以对private方法添加final修饰词，但这并不能给该方法添加任何额外的意义。

### final类
- final类无法被继承。
- final类禁止继承，所以final类中所有的方法都隐式指定为是fianl的，因为无法覆盖它们。在final类中可以给方法添加final修饰词，但这不会增添任何意义。

### 7.9 初始化及类的加载
- 每个类的编译代码都存在于它自己的独立的文件中。该文件只在需要使用程序代码时才会被加载。
- 通常加载发生于创建类的第一个对象之时，但是当访问static域或static方法时，也会发生加载。
- 初次使用之处也是static初始化发生之处。所有的static对象和static代码段都会在加载时依程序中的顺序而依次初始化。当然，定义为static的东西只会被初始化一次。

### 7.9.1 继承与初始化
```java
class Insect {
	private int i = 9;
	protected int j;

	Insect() {
		System.out.print("i = " + i + ", j = " + j);
		j = 39;
	}

	private static int x1 = printInit("static Insect.x1 initialized");

	static int printInit(String s) {
		System.out.print(s);
		return 47;
	}
}

public class Beetle extends Insect {
	private int k = printInit("Beetle.k initialized");

	public Beetle() {
		System.out.print("k = " + k);
		System.out.print("j = " + j);
	}

	private static int x2 = printInit("static Beetle.x2 initialized");

	public static void main(String[] args) {
		System.out.print("Beetle constructor");
		Beetle b = new Beetle();
	}
}
/*
 * Output: 
 * static Insect.x1 initialized 
 * static Beetle.x2 initialized 
 * Beetle constructor 
 * i = 9, j = 0 
 * Beetle.k initialized 
 * k = 47 
 * j = 39
 */
```
- 在Beetle上运行java时，所发生的第一件事就是试图访问Beetle.main()(一个static方法)。于是加载器开始启动并找出Beetle类的编译代码(在名为Beetle.class的文件之中)。在对它进行加载的过程中，编译器注意到他有一个基类（这是由关键字extends得知的），于是它继续进行加载。不管你是否打算产生一个该基类对象，这都要发生。
- 如果该基类还有其自身的基类 ，那么第二个基类就会被加载，如此类推。接下来，根基类中的static初始化（在此例中为Insect）即会被执行，然后是下一个导出类，以此类推。这种方式很重要，因为导出类的static初始化可能会依赖于基类成员能否被正确初始化。
- 至此为止，必要的类都加载完毕，对象就可以被创建了。
- 首先，对象中所有的基本类型都会被设为默认值，对象引用被设为null--这是通过将对象内存设为二进制零值而一举生成的。
- 然后，基类的构造器会被调用。在本例中，它是被自动调用的。但也可以用super来指定对基类构造器的调用（正如在Beetle()构造器中的第一步操作）。基类构造器和导出类的构造器一样，以相同的顺序来经历相同的过程。在基类构造器完成之后，实例变量按其次序被初始化。
- 最后构造器的其余部分被执行。

> 先加载类的static，然后开始对象的初始化。
