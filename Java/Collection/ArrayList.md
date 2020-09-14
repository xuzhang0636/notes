# ArrayList

## 1.什么是ArrayList

ArrayList是Java中非常常用的一种数据结构，如果你学过数据结构，可以把它当成一种增强的数组，帮助我们封装了数组的常用API，可以方便的去add,remove,iterator,etc..。

------

## 2.ArrayList的底层数据结构是什么

ArrayList的底层数据结构就是Object数组。

```java
    transient Object[] elementData;
```

既然它是数组，我们就要考虑数组的特性。

数组的特性：

* 如果知道index的话，查询很快。
* 删除和增加元素到指定的元素会很慢，因为要牵扯到数组中元素的移动。

所以说呢，需要知道各种数据结构的优缺点，这样才能让我们在用的时候可以进行选型。如果插入多的话，可以用LinkedList.如果查询多的话，可以用LinkedList.当然，LinkedList也有缺点，稍后在写文章进行记录。



------

## 3.为什么我们可以往里面无限丢数据

因为ArrayList有动态扩容机制，那么它是怎么样进行扩容的呢？在什么时候进行扩容？

你可以想一下，在什么时候进行扩容呢？当然是在添加元素的时候。可以从源码中看出

```java
public boolean add(E var1) {
  //扩容就在这一步
        this.ensureCapacityInternal(this.size + 1);
        this.elementData[this.size++] = var1;
        return true;
}
```

下面我们来详细的看一下这个ensureCapacityInternal(int)

```java
//里面调用了ensureExplicitCapacity,还有calculateCapacity
private void ensureCapacityInternal(int var1) {
        this.ensureExplicitCapacity(calculateCapacity(this.elementData, var1));
}

//首先我们看一下，如果当前为空数组，则默认给10，否则就是现在的大小也就是size+1,因为要加入一个元素，当然是size+1
private static int calculateCapacity(Object[] var0, int var1) {
        return var0 == DEFAULTCAPACITY_EMPTY_ELEMENTDATA ? Math.max(10, var1) : var1;
}

//可以看到，如果当前的size+1 > length，则需要调用grow方法进行扩容
private void ensureExplicitCapacity(int var1) {
        ++this.modCount;
        if (var1 - this.elementData.length > 0) {
            this.grow(var1);
        }
}
```





------

## 4.它是线程安全的么？如果不是，那么有什么方案来替代么？

------

## 5.