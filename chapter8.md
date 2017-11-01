# 第8章 多态

---
- 多态也称作动态绑定、后期绑定或运行时绑定
- Java中除了static方法和final方法（private方法属于final方法）之外，其他所有的方法都是后期绑定。
- 最好根据设计来决定是否使用final，而不是出于试图提高性能的目的来使用final
- 在导出类中，对于基类中的private方法，最好采用不同的名字。
- 多态中，子类无法覆盖父类的**公共属性和静态方法**。在实际中，这种情况从来不会发生。首先，你通常把域（属性）设置成private，因此不能直接访问他们，另外你可能不会对基类中的域和导出类中的域赋予相同的名字。示例：

```java
class Super {
	public int field = 0;

	public int getField() {
		return field;
	}
}

class Sub extends Super {
	public int field = 1;

	public int getField() {
		return field;
	}

	public int getSuperField() {
		return super.field;
	}
}

public class FieldAccess {
	public static void main(String[] args) {
		Super sup = new Sub(); // Upcast
		System.out.println("sup.field = " + sup.field + ", sup.getField() = " + sup.getField());
		Sub sub = new Sub();
		System.out.println("sub.field = " + sub.field + ", sub.getField() = " + sub.getField() + ", sub.getSuperField() = "
				+ sub.getSuperField());
	}
}

/*
 * Output: 
 * sup.field = 0, sup.getField() = 1 
 * sub.field = 1, sub.getField() = 1, sub.getSuperField() = 0
 */
```

### 构造器和多态
- 构造器不具有多态性（它们实际上是static方法，只不过该static声明是隐式的）
- 构造器调用要遵照下面的顺序：
	+ 调用基类构造器。这个步骤会反复递归下去
	+ 按声明顺序调用成员的初始化方法。
	+ 调用类构造器的主体。

> 在一个类的初始化中，会先初始化类的属性，再去执行构造器的主体内容。

```java
class Meal {
	Meal() {
		System.out.println("Meal()");
	}
	Meal(String s) {
		System.out.println(s);
	}
}

class Bread {
	Bread() {
		System.out.println("Bread()");
	}
}

class Cheese {
	Cheese() {
		System.out.println("Cheese()");
	}
}

class Lettuce {
	Lettuce() {
		System.out.println("Lettuce()");
	}
}

class Lunch extends Meal {
	private Meal meal = new Meal("ssss");
	Lunch() {
		System.out.println("Lunch()");
	}
}

class PortableLunch extends Lunch {
	PortableLunch() {
		System.out.println("PortableLunch()");
	}
}

public class Sandwich extends PortableLunch {
	private Bread b = new Bread();
	private Cheese c = new Cheese();
	private Lettuce l = new Lettuce();

	public Sandwich() {
		System.out.println("Sandwich()");
	}

	public static void main(String[] args) {
		new Sandwich();
	}
}
/*
 * Output: 
 * Meal() 
 * ssss
 * Lunch() 
 * PortableLunch() 
 * Bread() 
 * Cheese() 
 * Lettuce() 
 * Sandwich()
 */
```

### 构造器内部的多态方法行为
```java
class Glyph {
	void draw() {
		System.out.println("Glyph.draw()");
	}

	Glyph() {
		System.out.println("Glyph() before draw()");
		draw(); // 这里调用的子类的draw()方法
		System.out.println("Glyph() after draw()");
	}
}

class RoundGlyph extends Glyph {
	private int radius = 1;

	RoundGlyph(int r) {
		radius = r;
		System.out.println("RoundGlyph.RoundGlyph(), radius = " + radius);
	}

	void draw() {
		System.out.println("RoundGlyph.draw(), radius = " + radius);
	}
}

public class PolyConstructors {
	public static void main(String[] args) {
		new RoundGlyph(5);
	}
}
/* 
 * Output:
 * Glyph() before draw()
 * RoundGlyph.draw(), radius = 0
 * Glyph() after draw()
 * RoundGlyph.RoundGlyph(), radius = 5
 */

```
Glyph.draw()方法设计为要被覆盖，这种覆盖实在RoundGlyph中发生的。但是Glyph构造器会调用这个方法，结果导致了对RoundGlyph.draw()的调用，这看起来似乎是我们的目的。但是如果看到输出结果，我们发现当Glyph的构造器调用draw()方法时，radius不是默认初始化值1，而是0。  
前一节讲述的初始化顺序并不十分完整，而这正是解决这一谜题的关键所在。初始化的实际过程是：  
**1. 在任何事物发生之前，将分配给对象的存储空间初始化成二进制的零。  
2. 如前所述那样调用基类构造器。此时，调用被覆盖后的draw()方法（要在调用RoundGlyph构造器之前调用），由于步骤1的缘故，我们此时发现radius的值为0。    
3. 按照声明的顺序调用成员的初始化方法。  
4. 调用导出类的构造器主体。**

> 编写构造器时有一条有效的准则：“用尽可能简单的方法使对象进入正常状态；如果可以的话，避免调用其他方法”。在构造器内唯一能够安全调用的那些方法是基类中的final方法（也适用于private方法，它们自动属于final方法）。这些方法不会被覆盖，因此不会出现上述代码 的问题。

```java
class Actor {
	public void act() {
	}
}

class HappyActor extends Actor {
	public void act() {
		System.out.println("HappyActor");
	}
}

class SadActor extends Actor {
	public void act() {
		System.out.println("SadActor");
	}
}

class Stage {
	private Actor actor = new HappyActor();

	public void change() {
		actor = new SadActor();
	}

	public void performPlay() {
		actor.act();
	}
}

public class Transmogrify {
	public static void main(String[] args) {
		Stage stage = new Stage();
		stage.performPlay();
		stage.change();
		stage.performPlay();
	}
} /* Output:
HappyActor
SadActor
*///:~
```
- 一条通用的准则是：“用继承表达行为间的差异，并用字段表达状态上的变化”。上述例子中，两者都用到了：通过继承得到了两个不同的类，用于表达act()方法的差异；而Stage通过运用组合使自己的状态发生变化。这种情况下，这种状态的改变也就产生了行为的改变。

### 8.5.2 向下转型与运行时类型识别（RTTI）
```java
class Useful {
  public void f() {}
  public void g() {}
}

class MoreUseful extends Useful {
  public void f() {}
  public void g() {}
  public void u() {}
  public void v() {}
  public void w() {}
}	

public class RTTI {
  public static void main(String[] args) {
    Useful[] x = {
      new Useful(),
      new MoreUseful()
    };
    x[0].f();
    x[1].g();
    // Compile time: method not found in Useful:
    //! x[1].u();
    ((MoreUseful)x[1]).u(); // Downcast/RTTI
    ((MoreUseful)x[0]).u(); // Exception thrown ClassCastException
  }
}
```
