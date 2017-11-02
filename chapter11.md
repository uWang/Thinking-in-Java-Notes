# 第11章 持有对象

---
- removeAll(), retainAll()都是依赖于equals方法的。
- PriorityQueue上调用offer()方法来插入一个对象时，这个对象会在队列中被排序。默认的排序将使用对象在队列中的自然顺序，但你也可以通过提供自己的Comparator来修改这个顺序。
- PriorityQueue可以确保你调用peek()、poll()和remove()方法时，获取的元素将是队列中优先级最高的元素。