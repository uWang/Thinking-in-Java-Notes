# 第16章 数组

---
- 数组是一种效率最高的存储和随机访问对象引用序列的方式。
- System.arraycopy()不会执行自动包装和自动拆包，两个数组必须具有相同的确切类型。
- 数组比较可以用 Arrays.equals()方法。
- Arrays.sort()进行排序，Collections.reverseOrder()可以产生一个倒序的Comparator
- 如果想忽略大小写字母将单词都放在一起排序，可以使用String.CASE\_INSENSITIVE\_ORDER