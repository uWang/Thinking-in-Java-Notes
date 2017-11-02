# 第10章 内部类

---
- 通常，外部类将有一个方法，该方法返回一个指向内部类的引用。
- 内部类可以通过OuterClassName.InnerClassName访问。
- 内部类拥有其外围类的所有元素的访问权。
- 内部类里可以通过OuterClassName.this获得外部类的对象引用。
- .new语法示例，创建内部类对象：

```java
public class DotNew {
  public class Inner {}
  public static void main(String[] args) {
    DotNew dn = new DotNew();
    DotNew.Inner dni = dn.new Inner();
  }
}
```

- private内部类给类的设计者提供了一种途径，通过这种方式可以完全组织任何依赖类型的编码，并且完全隐藏了了实现的细节。从客户端程序员的角度看，由于不能访问任何新增加的、原本不属于公共接口的方法，所以扩展接口是没有价值的。

```java
class Parcel4 {
  private class PContents implements Contents {
    private int i = 11;
    public int value() { return i; }
  }
  protected class PDestination implements Destination {
    private String label;
    private PDestination(String whereTo) {
      label = whereTo;
    }
    public String readLabel() { return label; }
  }
  public Destination destination(String s) {
    return new PDestination(s);
  }
  public Contents contents() {
    return new PContents();
  }
}

public class TestParcel {
  public static void main(String[] args) {
    Parcel4 p = new Parcel4();
    Contents c = p.contents();
    Destination d = p.destination("Tasmania");
    // Illegal -- can't access private class:
    //! Parcel4.PContents pc = p.new PContents();
  }
}
```

- 可以在方法内部或者作用域里创建内部类。
- 如果定义了一个匿名内部类，并且希望它使用一个外部定义的对象，那么编译器会要求其参数引用是fianl的。
- 匿名内部类与正规的继承相比有些受限，因为匿名内部类既可以扩展类，也可以实现接口，但不能两者兼备。而且如果是实现接口，也只能实现一个接口。

### 10.7 嵌套类
- 如果不需要内部类对象与外围类对象之间有联系，那么可以将内部类声明为static，这通常称为嵌套类。
- 普通的内部类对象隐式的保存了一个引用，指向外部类对象。
- 嵌套类意味着：
	+ 要创建嵌套类的对象，并不需要外围类的对象。
	+ 不能从嵌套类的对象中访问非静态的外围类对象。
	+ 普通内部类不能有static数据和static字段，也不能包含嵌套类。但嵌套类可以包含所有东西
- 接口中使用内部类示例

```java
public interface ClassInInterface {
  void howdy();
  class Test implements ClassInInterface {
    public void howdy() {
      System.out.println("Howdy!");
    }
    public static void main(String[] args) {
      new Test().howdy();
    }
  }
}
```

### 10.8 为什么需要内部类
- 每个内部类都能独立地继承自一个（接口的）实现，所以无论外围类是否已经继承了某个（接口的）实现，对于内部类都没有影响。
- 内部类解决了**“多重继承”**的问题
	+ 内部类可以有多个实例，每个实例都有自己的状态信息，并且与其他外围类对象的信息相互独立。
	+ 在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或继承同一个类。
	+ 创建内部类对象的时刻并不依赖于外围类对象的创建。
	+ 内部类并没有令人迷惑的“is-a”关系，他就是一个独立的实体。

### 10.8.1 之后缺少应用场景的实践，不太理解作者的意思。

### 10.9 内部类的继承
- 内部类的继承，必须要在构造器内使用以下语法：`enclosingClassReference.super()`

```java
class WithInner {
  class Inner {}
}

public class InheritInner extends WithInner.Inner {
  //! InheritInner() {} // Won't compile
  InheritInner(WithInner wi) {
    wi.super();
  }
  public static void main(String[] args) {
    WithInner wi = new WithInner();
    InheritInner ii = new InheritInner(wi);
  }
}
```

### 10.10 内部类可以被覆盖吗
- 内部类不会被覆盖，他们是两个独立的实体，各自在自己的命名空间里。