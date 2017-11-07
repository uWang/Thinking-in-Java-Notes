# 第17章 容器深入研究

---
- Collections.fill()和Collections.nCopies()方法用于复制同一个对象来填充整个容器，并且只对List对象有用。
- Arrays.asList()会产生一个List，它基于一个固定大小的数组，仅支持那些不会改变数组大小的操作。任何会引起对底层数据结构的尺寸进行修改的方法都会产生一个UnsupportedOperationException异常，以表示对未获支持操作的调用（一个编程错误）
- Set的一些要求

|      类名      |                            说明                            |
|----------------|------------------------------------------------------------|
| Set(interface) | 加入Set的元素必须定义equals()保证对象的唯一性              |
| HashSet        | 为快速查找而设计的Set。存入HashSet的元素必须定义HashCode() |
| TreeSet        | 保持次序的Set，底层为树结构，元素必须实现Comparable接口    |
| LinkedHashSet  | 具有HashSet的查询速度，且内部使用链表维护元素的顺序，元素必须定义hashCode()方法                                                           |

- HashSet应该是你的默认选择，因为它对速度进行了优化。
- 对于良好的编程风格而言，覆盖equals()方法时，总是同时覆盖hashCode()方法。

- Map的一些特性

|      类名     |                                                                       说明                                                                       |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| HashMap       | 无其他特性下的默认选择，取代了HashTable                                                                                                          |
| LinkedHashMap | 类似于HashMap，但是遍历它时是按插入次序的或者是最近最少使用LRU的次序。                                                                           |
| TreeMap       | 基于红黑树的实现。元素会被排序，由Comparable或者Comparator决定。其特点是结果是经过排序的。TreeMap是唯一带有subMap()方法的Map，它可以返回一个子树 |

- 正确的equals()方法必须满足下列5个条件：
	+ 自反性。对任意x，x.equals(x)一定返回true
	+ 对称性。对任意x和y，如果y.equals(x)返回true，则x.equals(y)也返回true
	+ 传递性。对任意的x、y、z，如果有x.equals(y)返回true，y.equals(z)返回true，则x.equals(z)一定返回true
	+ 一致性。对任意x和y，如果对象中用于等价比较的信息没有改变，那么无论调用x.equals(y)多少次，返回的结果应该保持一致，要么一直是true，要么一直是false
	+ 对任何不是null的x，x.equals(null)一定返回false。