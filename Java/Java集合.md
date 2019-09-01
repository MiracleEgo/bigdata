# Collection

## List

### ArrayList

* 实现List接口，底层使用数组保存所有元素，操作基本上是对数组的操作a

  ```java
  transient Object[] elementData; 
  ```

* ArrayList三种构造器

  * 默认空列表，初始容量为10
    * 若没指定ArrayList大小，第一次调用add时，会初始化数组容量为10
  * 指定初始容量的空列表
  * 指定collection元素的列表，按照collection迭代器返回它们的顺序排列

* 存储

  * `add(E e)`
    * 将指定元素添加至列表尾
    * `ensureCapacityInternal()`会检查添加后元素个数是否会超过当前数组长度，若超出则扩容
  * `add(int index,E element)`
  * addAll(Collection<? extends E> c)

* 扩容

  * 将老数组无数拷贝进新数组，容量为原容量的1.5倍，实际使用应避免数组扩张
  * 当可预知保存元素数量时，在构造ArrayList时就指定容量，以避免扩容，或根据实际需求，通过调用`ensureCapacity()`手动增加容量

* 线程不安全

  * 故障现象`java.util.ConcurrentModificationException`并发修改异常
  * 导致原因
    * 
  * 解决方案
    * 使用`Vector`
    * `Collections.synchronizedList(new ArrayList<>());`
    * 写时复制`new CopyOnWriteArrayList<>();`
      * 往一个 容器添加元素时不直接在容器添加，而是将当前容器Object[]进行copy复制出一个新容器`Object[] newElements`,然后在新容器中添加元素，添加完成后，将原容器引用指向新粉吕`setArray(nweElements)`
      * 好处是可对容器进行并发读而不需加锁，读写分离的思想

### LinkedList

### Vector

## Set

* HashSet
  * 保证元素唯一性
* TreeSet
  * 保证元素排序

## Queue

# Map

## HashMap

HashMap 底层是基于 **`数组 + 链表`** 组成的，不过在 jdk1.7 和 1.8 中具体实现稍有不同。

![img](https://i.loli.net/2019/05/08/5cd1d2be77958.jpg)

 HashMap 中比较核心的几个成员变量；

1. 初始化桶大小，因为底层是数组，所以这是数组默认的大小。
2. 桶最大值。
3. 默认的负载因子（0.75）
4. `table` 真正存放数据的数组。
5. `Map` 存放数量的大小。
6. 桶大小，可在初始化时显式指定。
7. 负载因子，可在初始化时显式指定。

给定的默认容量为 16，负载因子为 0.75。Map 在使用过程中不断的往里面存放数据，当数量达到了 `16 * 0.75 = 12` 就需要将当前 16 的容量进行扩容，而**扩容这个过程涉及到 rehash、复制数据等操作，所以非常消耗性能**。

因此通常建议能提前预估 HashMap 的大小最好，尽量的减少扩容带来的性能损耗。

### 1.7 HashMap缺点

> 当 Hash 冲突严重时，在桶上形成的链表会变的越来越长，这样在查询时的效率就会越来越低；时间复杂度为 `O(N)`。

因此 1.8 中使用了数组+链表+红黑树

###  1.8 HashMap结构

![img](https://i.loli.net/2019/05/08/5cd1d2c1c1cd7.jpg)

原理上来说：ConcurrentHashMap 采用了分段锁技术，其中 Segment 继承于 ReentrantLock。不会像 HashTable 那样不管是 put 还是 get 操作都需要做同步处理，理论上 ConcurrentHashMap 支持 CurrencyLevel (**Segment 数组数量**)的线程并发。每当一个线程占用锁访问一个 Segment 时，不会影响到其他的 Segment。

### HashMap的扩容

**当map中包含的Entry的数量大于等于threshold = loadFactor \* capacity的时候，且新建的Entry刚好落在一个非空的桶上，此刻触发扩容机制，将其容量扩大为2倍。**

**旧桶数组中的某个桶的外挂单链表是通过头插法插入新桶数组中的，并且原链表中的Entry结点并不一定仍然在新桶数组的同一链表**。

**HashMap的容量必须是2的幂**，那么为什么要这么设计呢？答案当然是为了性能。在HashMap通过键的哈希值进行定位桶位置的时候，调用了一个indexFor(hash, table.length);方法。

```java
/**
 * Returns index for hash code h.
 */
static int indexFor(int h, int length) {
    return h & (length-1);
}
```
可以看到这里是将哈希值h与桶数组的length-1（实际上也是map的容量-1）进行了一个与操作得出了对应的桶的位置，h & (length-1)。

但是为什么不采用h % length这种计算方式呢？

https://bugs.java.com/bugdatabase/view_bug.do?bug_id=4631373中提出Java的%、/操作比&慢10倍左右，因此**采用&运算会提高性能**。

**通过限制length是一个2的幂数，h & (length-1)和h % length结果是一致的。这就是为什么要限制容量必须是一个2的幂的原因**

### put方法的逻辑

* 如果HashMap未被初始化过，则初始化
* 对key求Hash值，然后再计算下标
* 如果没有碰撞，直接放入桶中
* 如果碰撞了，以链表的方式链接到后面
* 如果链表长度超过阀值，就把链表转成红黑树
* 如果链表长度低于6，就把红黑树转回链表
* 如果节点已经存在就替换旧值
* 如果桶满了（容量16*负载因子0.75），就需要resize（扩容2倍后重排）

## **Hashtable与HashMap的区别**

HashMap不是线程安全的，HashTable是线程安全。

HashMap允许空（null）的键和值（key），HashTable则不允许。

HashMap性能优于Hashtable。

Map

1. Map是一个以键值对存储的接口。Map下有两个具体的实现，分别是HashMap和HashTable.
2. HashMap是线程非安全的，HashTable是线程安全的，所以HashMap的效率高于HashTable.
3. HashMap**允许键或值为空**，而HashTable不允许键或值为空.



## HashSet

对于 HashSet 而言，它是基于 HashMap 实现的，底层采用 HashMap 来保存元素

由于 HashMap 的 put() 方法添加 key-value 对时，当新放入 HashMap 的 Entry 中 key 与集合中原有 Entry 的 key 相同（hashCode()返回值相等，通过 equals 比较也返回 true），新添加的 Entry 的 value 会将覆盖原来 Entry 的 value（HashSet 中的 value 都是`PRESENT`），但 key 不会有任何改变，因此**如果向 HashSet 中添加一个已经存在的元素时，新添加的集合元素将不会被放入 HashMap中，原来的元素也不会有任何改变，这也就满足了 Set 中元素不重复的特性。**

## ConcurrentHashMap

* Java5 使用segment分段锁
* java8 CAS+synchronized使锁更细化
* 数组+链表+红黑树

### put方法的逻辑

* 如果key或者value为null，则抛出空指针异常； 
* 如果table为null或者table的长度为0，则初始化table，调用initTable()方法。
* 通过hash定位数组索引坐标，是否有Node节点，如果没有，使用CAS进行添加（链表的头节点，添加失败则进入下次循环）
* 如果当前位置的节点元素的hash值为-1，说明这是一个ForwaringNodes节点，即正在进行扩容。那么当前线程加入扩容。 
* 如果f!=null，则使用synchronized锁住f元素（链表/红黑二叉树的头元素）
  * 如果是Node（链表结构），则执行链表的添加操作
  * 如果是TreeNode（树型结构）则执行树添加操作
* 判断链表长度已经达到临界值8，当节点数超过这个值就需要把链表转换为树结构

### get方法的逻辑

- ConcurrentHashMap的get方法就是从Hash表中读取数据，而且与扩容不冲突。该方法没有同步锁。 
- 通过键值的hash计算索引位置，如果满足条件，直接返回对应的值； 
- 如果相应节点的hash值小于0 ，即该节点在进行扩容，直接在调用ForwardingNodes节点的find方法进行查找。 
- 否则，遍历当前节点直到找到对应的元素。

**总结**

* 首先使用无锁操作CAS插入头节点，失败则循环重试
* 若头节点已存在，则尝试获取头节点的同步锁，再进行操作



# HashMap、HashTable、ConcurrentHashMap的区别

* HashMap线程不安全，数组+链表+红黑树
* HashTable线程案例。锁住整个对象，数组+链表
* ConcurrentHashMap线程安全，CAS+同步锁，数组+链表+红黑树
* HashMap的key和value可以为null，其他两个不行

# Array与ArrayList的区别