# 第14章 类型信息

---
- Java使用Class对象来执行其RTTI。
- 所有的类都是在对其第一次使用时，动态加载到JVM中的。
- 一旦某个类的Class对象被载入内存，他就被用来创建这个类的所有对象。
- 普通的类引用不会产生警告，你可以看到，尽管泛型类引用只能赋值为指向其声明的类型，但是普通的类引用可以被重新赋值为指向任何其他的Class对象。通过泛型语法，可以让编译器强制执行额外的类型检查。

```java
public class GenericClassReferences {
  public static void main(String[] args) {
    Class intClass = int.class;
    Class<Integer> genericIntClass = int.class;
    genericIntClass = Integer.class; // Same thing
    intClass = double.class;
    // genericIntClass = double.class; // Illegal
  }
}
```

- 使用通配符“?”,可以放松限制：

```java
public class WildcardClassReferences {
  public static void main(String[] args) {
    Class<?> intClass = int.class;
    intClass = double.class;
  }
}
```

```java
public class BoundedClassReferences {
  public static void main(String[] args) {
    Class<? extends Number> bounded = int.class;
    bounded = double.class;
    bounded = Number.class;
    // Or anything else derived from Number.
  }
} 
```

- instanceof和inInstance()生成的结果完全一样，都指的是“你是这个类吗，或者你是这个类的派生类吗？”
- ==和equeals比较实际的Class对象，就没有考虑继承--它或者是这个确切的类型或者不是。

- Class类与java.lang.reflect类库一起对反射的概念进行支持。这些类型的对象是由JVM在运行时创建的，用以表示未知类里对应的成员。这样你就可以使用Constructor创建新的对象，用get()和set()方法读取和修改与Field对象相关联的字段，用invoke()方法调用与Method对象相关联的方法。调用getFields()、getMethods()和getConstructors()等。

### 动态代理
- 简单的代理示例：

```java
interface Interface {
	void doSomething();

	void somethingElse(String arg);
}

class RealObject implements Interface {
	public void doSomething() {
		System.out.println("doSomething");
	}

	public void somethingElse(String arg) {
		System.out.println("somethingElse " + arg);
	}
}

class SimpleProxy implements Interface {
	private Interface proxied;

	public SimpleProxy(Interface proxied) {
		this.proxied = proxied;
	}

	public void doSomething() {
		System.out.println("SimpleProxy doSomething");
		proxied.doSomething();
	}

	public void somethingElse(String arg) {
		System.out.println("SimpleProxy somethingElse " + arg);
		proxied.somethingElse(arg);
	}
}

class SimpleProxyDemo {
	public static void consumer(Interface iface) {
		iface.doSomething();
		iface.somethingElse("bonobo");
	}

	public static void main(String[] args) {
		consumer(new RealObject());
		consumer(new SimpleProxy(new RealObject()));
	}
}/* Output:
doSomething
somethingElse bonobo
SimpleProxy doSomething
doSomething
SimpleProxy somethingElse bonobo
somethingElse bonobo
*/
```

- 动态代理示例：

```java
import java.lang.reflect.*;

class DynamicProxyHandler implements InvocationHandler {
	private Object proxied;

	public DynamicProxyHandler(Object proxied) {
		this.proxied = proxied;
	}

	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		System.out.println("**** proxy: " + proxy.getClass() + ", method: " + method + ", args: " + args);
		if (args != null)
			for (Object arg : args)
				System.out.println("  " + arg);
		return method.invoke(proxied, args);
	}
}

public class SimpleDynamicProxy {
	public static void consumer(Interface iface) {
		iface.doSomething();
		iface.somethingElse("bonobo");
	}

	public static void main(String[] args) {
		RealObject real = new RealObject();
		consumer(real);
		// Insert a proxy and call again:
		Interface proxy = (Interface) Proxy.newProxyInstance(Interface.class.getClassLoader(),
				new Class[] { Interface.class }, new DynamicProxyHandler(real));
		consumer(proxy);
	}
} /* Output: (95% match)	
doSomething
somethingElse bonobo
**** proxy: class $Proxy0, method: public abstract void Interface.doSomething(), args: null
doSomething
**** proxy: class $Proxy0, method: public abstract void Interface.somethingElse(java.lang.String), args: [Ljava.lang.Object;@42e816
  bonobo
somethingElse bonobo
*/
```
调用静态方法Proxy.newProxyInstance()可以创建动态代理，这个方法需要一个类加载器，一个你希望该代理实现的接口列表，以及InvocationHandler接口的一个实现。

### 空对象
不太清除应用场景

### 接口与类型信息
- 可以通过RTTI，将接口对象转型为其实现类，从而获取实现类更多丰富的方法。

```java
interface A {
  void f();
}
class B implements A {
  public void f() {}
  public void g() {}
}

public class InterfaceViolation {
  public static void main(String[] args) {
    A a = new B();
    a.f();
    // a.g(); // Compile error
    System.out.println(a.getClass().getName());
    if(a instanceof B) {
      B b = (B)a;
      b.g();
    }
  }
} /* Output:
B
*/
```

- 也可以通过反射进行隐藏方法的调用

```java
class C implements A {
  public void f() { print("public C.f()"); }
  public void g() { print("public C.g()"); }
  void u() { print("package C.u()"); }
  protected void v() { print("protected C.v()"); }
  private void w() { print("private C.w()"); }
}

class HiddenC {
  public static A makeA() { return new C(); }
}

public class HiddenImplementation {
  public static void main(String[] args) throws Exception {
    A a = HiddenC.makeA();
    a.f();
    System.out.println(a.getClass().getName());
    // Compile error: cannot find symbol 'C':
    /* if(a instanceof C) {
      C c = (C)a;
      c.g();
    } */
    // Oops! Reflection still allows us to call g():
    callHiddenMethod(a, "g");
    // And even methods that are less accessible!
    callHiddenMethod(a, "u");
    callHiddenMethod(a, "v");
    callHiddenMethod(a, "w");
  }
  static void callHiddenMethod(Object a, String methodName)
  throws Exception {
    Method g = a.getClass().getDeclaredMethod(methodName);
    g.setAccessible(true);
    g.invoke(a);
  }
}
```