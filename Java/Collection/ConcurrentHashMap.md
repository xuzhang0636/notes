## 1.它为什么会出现

众所周知，HashMap是线程不安全的。如果我们程序中有大量的高并发操作，要么使用HashTable(不推荐)，要么使用Collections.synchronizedMap()来获取一个线程安全的Map。但是上面两个的并发效率不是太好。所以在JUC下面给我们提供了ConcurrentHashMap这个类来帮助我们有线程安全的Map类使用。

------

## 2.Java1.7和Java1.8的区别

**Java1.7**使用的是分段锁机制，即Segment。Segment是一个可重用的锁，它里面封装了HashEntry的数组，而且，HashEntry又是一个链表结构，其实说白了，还是使用的数组加链表的结构。

**Java1.8**对数据结构进行了重构，去除了Segment结构，

------

## 3.它是如何保证高并发的