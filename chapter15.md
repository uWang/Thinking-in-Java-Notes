第15章 泛型

---
- 泛型的主要目的之一就是用来指定容器要持有什么类型的对象，而且由编译器来保证类型的正确性。

- 斐波那契数列

```java
interface Generator<T> {
	T next();
}

public class Fibonacci implements Generator<Integer> {
	private int count = 0;

	public Integer next() {
		return fib(count++);
	}

	private int fib(int n) {
		if (n < 2)
			return 1;
		return fib(n - 2) + fib(n - 1);
	}

	public static void main(String[] args) {
		Fibonacci gen = new Fibonacci();
		for (int i = 0; i < 18; i++)
			System.out.print(gen.next() + " ");
	}
} /*
	 * Output: 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584
	 */// :~
```

- 擦除特性存在的正当理由是从非泛化代码到泛化代码的转变过程，以及不破坏现有类库的情况下，将泛型融入Java语言。

- 特性比较多，以后继续补充
