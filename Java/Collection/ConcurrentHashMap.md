## 1.它为什么会出现

众所周知，HashMap是线程不安全的。如果我们程序中有大量的高并发操作，要么使用HashTable(不推荐)，要么使用Collections.synchronizedMap()来获取一个线程安全的Map。但是上面两个的并发效率不是太好。所以在JUC下面给我们提供了ConcurrentHashMap这个类来帮助我们有线程安全的Map类使用。

------

## 2.Java1.7和Java1.8的区别

**Java1.7**使用的是分段锁机制，即Segment。Segment是一个可重用的锁，它里面封装了HashEntry的数组，而且，HashEntry又是一个链表结构，其实说白了，还是使用的数组加链表的结构。

**Java1.8**对数据结构进行了重构，去除了Segment结构，

------

## 3.它是如何保证高并发的

### 3.1 Java1.7

上面简单地介绍了一下Java1.7的分段锁机制，在这我们再次回顾一下Java1.7中的机制。

因为Segment本省继承了ReetranLock,所以，它本身就是一个锁，Segment内部维护了一个HashEntry数组，而且HashEntry本身就是一个链表的结构。那么你就可以把它想象成其实每一个Segment都是一个HashMap,而且，当你put一个值的时候，会进入到一个Segment，这个时候，该Segment会进行上锁，但是，它不会影响其他的Segment，所以呢，效率大大地进行了提高。

### 3.2Java8

运用头插法的形式，并且利用了CAS和锁住桶的头部，使锁的粒度更加的细粒度。