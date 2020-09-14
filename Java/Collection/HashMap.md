# HashMap

## HashMap的数据结构是什么？

HashMap的底层数据结构是数组加链表的形式。

![image-20200914191446846](./HashMap.assets/image-20200914191446846.png)

在Java1.7种用的是Entry,在Java1.8种变成了Node.

在HashMap插入的时候，首先会利用hash(key)函数进行hash计算，如果计算的结果，你属于一号桶，则将key,value放入到一号桶，那为什么还有链表呢？这是因为可能在hash(key)的时候，两个key的hash以后的结果是一样的，比如都到了一号桶，这个时候，在1.8种会利用尾插法将key,value插入到一号桶中，里面以链表的形式进行存储。当然，如果相同的key值，则会将原来的value覆盖。

而且，在Java1.8中，还对链表进行了优化，如果链表的个数大于7，则会将链表变成红黑树，如果这个时候，有删除操作，当链表个数小于7，则会将红黑树变成链表。有相互转换的过程。这个7就是临界点。**至于为什么要用7，我得再研究研究**。

------

## HashMap的扩容机制

什么时候HashMap会进行扩容呢？

有两个因素会影响HashMap的扩容，一个是HashMap中桶的容量，一个就是这个负载因子

```java
static final float DEFAULT_LOAD_FACTOR = 0.75f;

```

举个例子，如果现在容量是100，当其中的元素大雨75的时候，HashMap就会进行resize.

resize的步骤，两步：

1.扩容为原来的两倍

2.将原来的元素重新进行hash,因为桶的数量变了，所以我们需要重新计算hash,为了保证值能均匀地插入每一个桶中。

------

## 头插和尾插

Java1.7中使用的是头插法，但是当多线程的时候会有死循环的可能。

在Java1.8中将其优化成了尾插法，避免了死循环的可能。

为什么尾插法可以有效的避免死循环？会写另一篇文章来说明其中的门道。

------

## HashMap是线程安全的么？如何保证线程安全

------

HashMap不是线程安全的，有三种方法，第一种，使用HashTable,第二种，使用Collections.synchronizedMap(new HashMap<>())，第三种，使用ConcurrentHashMap.