# Java集合类

Owner: Terrence
Verification: Verified
Tags: Java
Created time: January 18, 2024 12:13 AM

## 1.1 Java中有哪些容器（集合类）？

Java中的集合类主要由Collection和Map这两个接口派生出，Collection又派生出三个子接口，分别是Set、List、Queue。

所有的Java集合类都是Set、List、Queue、Map的实现类。

Set：无序的，元素不可重复的集合

List：有序的，元素可以重复的集合

Queue：代表先进先出（FIFO）的队列

Map：代表具有映射关系（key-value）的集合，所有的key是一个set集合

Collection

- Set
    - EnumSet
    - SortedSet
        - TreeSet
    - HashSet
        - LinkedHashSet
- Queue
    - Deque
        - ArrayDeque
    - PriorityQueue
- List
    - LinkedList
    - ArrayList
    - Vector
        - Stack

Map

- EnumMap
- IdentityHashMap
- HashMap：线程不安全，key、value允许为nul
    - LinkedHashMap
- HashTable：线程安全，key、value不能为nul
    - Properties
- SortedMap
    - TreeMap
- WeakHashMap

## 1.2 Java中的容器，线程安全和线程不安全的有哪些？

java.util包下的集合类大部分都是线程不安全的，比如HashSet、TreeSet、ArrayList、LinkedList、ArrayDeque、HashMap、TreeMap，优点是性能好

线程不安全（性能好）

- java.util
    - HashSet
    - TreeSet
    - ArrayList
    - LinkedList
    - ArrayDeque
    - HashMap
    - TreeMap

java.util.concurrent包下提供大量支持高效并发访问的集合

线程安全

- 以Concurrent开头的集合类
    - 支持并发访问的集合，支持多个线程并发写入访问，写入线程的所有操作都是线程安全的，读取操作不必锁定。
- 以CopyOnWrite开头的集合类
    - 采用复制底层数组的方式实现写入操作。当线程读取这类集合，线程会直接读取集合本身，无需加锁和阻塞。当线程对这类集合执行写入操作时，集合会在底层复制一份新的数组，对新的数组执行写入。

## 1.3 Map接口的实现类

HashMap, LinkedHashMap, TreeMap, ConcurrentHashMap

不需要排序，优先使用HashMap。

线程安全，使用ConcurrentHashMap，它的性能好于Hashtable， 因为在put时采用分段锁/CAS的加锁机制，而不是像Hashtable一样，无论put还是get都做同步处理。

需要按插入顺序排序，使用LinkedHashMap。

需要将key自然顺序排序和自定义顺序排序，TreeMap。

## 1.4 Map put的过程

1. 调用`put(key, value)` 后，首先计算key的hashCode()。
2. 转换hashCode为数组索引
3. 处理hash冲突
    - 如果没有元素，直接放入key和value
    - 如果有元素
        - 如果key和当前插入的key相同，更新value
        - 如果不同，则以链表或红黑树存储新的键值对

添加新元素时，还会考虑是否超过容量和threshold，超过了会resize

## 1.5 如何得到线程安全的Map？

1. 使用Collections.synchronizedMap()，可以将普通的Map包装成线程安全的Map
    
    ```java
    Map<K, V> map = new HashMap<>();
    Map<K, V> synchronizedMap = Collections.synchronizedMap(map);
    
    synchronized(synchronizedMap) {
    	for(Map.Entry<K, V> entry : synchronizedMap.entrySet()) {
    		// ...
    	}
    }
    ```
    
    这会在每个Map方法上加上同步块来实现线程安全。
    
2. 使用ConcurrentHashMap，是Java并发包提供的一个线程安全的Map实现。
    
    ```java
    ConcurrenrtMap<K, V> concurrentHashMap = new ConcurrentHashMap<>();
    ```
    
    使用分段锁来减少锁竞争，使得在高并发环境下的性能比普通的同步Map更好。
    
3. 使用Hashtable，线程安全的Map
    
    ```java
    Map<K, V> table = new Hashtable<>();
    ```
    
    所有操作都是同步的，多线程环境中不如ConcurrentHashMap
    

## 1.6 HashMap特点

1. HashMap是线程不安全的实现
2. HashMap可以使用null作为key或value

### JDK7和JDK8中的HashMap有什么区别？

JDK7

- 数组+链表
- 底层维护一个Entry数组，根据计算的hashCode将对应的KV键值对存储到该数组中，一旦发生hashCode冲突，就会将KV放到对应的已有元素的后面，链表
- 当Hash冲突严重的时候，在桶上形成的链表会越来越长，会接近O(N)

JDK8

- 数组+链表+红黑树
- 底层维护一个Node数组，当链表的存储个数大于8的时候，使用红黑树。优化性能，O(N)⇒O(logN)

### HashMap底层实现原理

存储对象时

- 调用Key的hashCode计算hash得到bucket位置，进一步存储，HashMap会根据当前bucket的占用情况自动调整容量。

获取对象时

- 将Key传给get， 调用hashCode计算hash得到bucket位置，调用equals()确定键值对

### HashMap扩容机制

1. 数组的初始容量为16，容量以2的次方扩充，位运算代替取模运算（更快）
2. 数组是否需要扩充是通过loading factor判断的，如果当前元素个数为容量的0.75时，会扩充。0.75为默认loading factor，可以设置大于1的loading factor，这样数组不会扩充，牺牲性能，节约内存。
3. 为了解决碰撞，数组中的元素时单向链表类型。当链表长度到达阈值（通常为8），会将链表转换为红黑树提高性能，在这之前还会先检测当前数组是否到达一个阈值（64），如果没有到达这个容量，会先放弃转换，选择扩充数组。当链表长度缩小到另一个阈值（6），又会将红黑树转换为单向链表提高性能。

### HashMap的循环链表

HashMap会使用链表来处理冲突的元素。

在多线程环境下，如果两个或多个线程同时对同一个HashMap进行写操作（比如插入），并且这个HashMap需要扩容，那么这些线程会同时修改同一个链表。

在调整大小的过程中，存储在链表中的元素的次序会反过来，因为移动到新的Bucket位置的时候，HashMap并不会将元素放在链表的尾部，而是放在头部，为了避免尾部遍历。

由于HashMap内部并没有线程同步机制，所以这种同时修改会导致链表中的元素互相引用，形成一个闭环（循环链表）。

**HashMap为什么线程不安全？因为在并发执行put操作时，可能会导致形成循环链表，引起死循环**

### HashMap为什么用红黑树而不是B树

HashMap本来是数组+链表的形式，链表由于其查找慢的特点，所以需要树结构来代替。

红黑树

- 自平衡二叉树，保证树的最大高度为`2*log(N+1)`，在最坏情况下查询为`O(logn)`
- 插入删除查找效率很高。插入和删除时，只需要进行少量颜色变换和不超过三次树旋转，就可以重新平衡。
- 每个节点只需要存储颜色，左右子节点和父节点引用。内存占用更高效

B树

- 高度平衡
- B树设计适合用于存储在外部存储器上的大量数据，因为他减少了访问外存的次数
- 每个节点可以有多个字节点和键值对，存储大量数据时B树高度较低

在HashMap中，一般情况下，每个哈希桶的冲突数量不会非常大，红黑树在处理相对较小的数据集时非常高效。红黑树节点结构更简单，适合在内存中频繁操作。HashMap需要快速的插入、删除和查找。

如果用B/B+树的话，在数据量不是很多的情况下，数据都会挤在一个节点里，这个时候遍历效率就会退化成链表。

### 线程安全HashMap

1. Hashtable
2. ConcurrentHashMap
3. 用Collections将HashMap包装成线程安全的Map

### HashMap和Hashtable的区别

1. Hashtable时线程安全的Map实现，HashMap是线程不安全的实现。
    
    性能：HashMap>Hashtable
    
2. Hashtable不允许使用null作为key和value，如果把null放入Hashtable，会引发空指针异常。
    
    HashMap可以使用null作为key和value
    

### HashMap和ConcurrentHashMap的区别

1. HashMap非线程安全，并发插入元素的时候会导致链表成环。Collections可以将Map转换成线程安全的实现，通过包装类，把所有功能委托给传入的Map。包装类是基于synchronized关键字来保证线程安全，底层使用互斥锁，性能和吞吐量比较低。
2. ConcurrentHashMap性能要高上许多，没有使用全局锁来锁住自己，而是采用减少锁粒度的方法，尽量减少因为竞争锁而导致的阻塞和冲突，而且ConcurrentHashMap的检索操作是不需要锁的。

## 1.7 ConcurrentHashMap

### ConcurrentHashMap是怎么实现的？

1. 分段锁（Segmentation）
    - ConcurrentHashMap内部维护了一个segment数组，每个segment是一个专门化的`HashMap`，具有自己的哈希表数组和计数器等，并且每个segment独立锁定，当线程需要访问segment中的数据时，必须先获得对应的锁，这意味着线程可以同时更新`ConcurrentHashMap`中的不同段，减少线程间的竞争
2. 数据结构
    - 每个segment内部结构和`HashMap`类似，即数组+链表的形式。当发生哈希冲突时，新的元素会被加入到链表的头部
3. 并发操作
    - 读：不需要加锁，因为即使读取过程中数据结构发生变化，读取的数据也是一致的。
    - 写：需要对特定的segment加锁。只有当对同一个segment进行操作时，写操作才会互相阻塞。
4. 扩容
    - 对segment独立进行扩容，局部扩容。
    
    ![IMG_2944.PNG](Java%E9%9B%86%E5%90%88%E7%B1%BB%206db6962519814f64bc03138dba8a4522/IMG_2944.png)
    

### ConcurrentHashMap分段分组

1. get操作
    - 先经过一次再散列，使用这个散列值定位到具体的segment，然后再通过散列算法定位到元素。
    - 整个get过程都不需要加锁，除非读到空的值才会加锁重读。原因就是将使用的共享变量定义成`volatile`类型。
    - `volatile` 类型：指示编译器和运行时将这个变量从主内存中读取，而不是从线程的工作内存中读取。
2. put操作
    1. 判断是否需要扩容
    2. 定位到添加元素的位置，将其放入HashEntry数组中
    
    插入过程会进行第一次key的hash来定位segment的位置，如果该segment还没有初始化，通过CAS操作来赋值，然后进行第二次hash操作，找到对应的HashEntry位置，在将数据插入制定的HashEntry时，会通过继承ReentrantLock的`tryLock()`方法尝试获取锁，如果成功就插入对应的位置，如果已经有线程获取该segment的锁，就会以自旋的方式继续调用`tryLock()`，超过指定次数就会挂起，等待唤醒。
    

## 1.8 LinkedHashMap

基于`HashMap`实现，并且内部维护了一个双向链表

特性

- 保持插入顺序，双向链表维护了键值对的插入顺序，使遍历`LinkedHashMap`时元素的返回顺序和插入顺序一致
- 性能：与HashMap性能相似，插入删除定位和遍历，时间复杂度为O(1)
- 内存：维护了额外的双向链表，内存占用比HashMap高

应用场景

- LinkedHashMap可以配置为基于访问顺序，适合用来实现缓存策略，LRU缓存

双向链表

- 每个元素在LinkedHashMap中都是一个节点
- 每个节点除了HashMap的哈希表所需信息外，还存储了previous和next的引用

插入顺序

- 每当插入新元素，它会被添加在双向链表的末尾
- 如果一个元素被重新插入（已存在该Key），在链表中的位置不会改变

访问顺序

- LinkedHashMap接受一个布尔参数，制定是否按访问顺序排列元素，如果设置为true，那么最近访问元素会被移动到双向链表的末尾
- 访问：`put`, `get`, `putAll`等操作

```java
LinkedHashMap<Integer, Integer> map = new LinkedHashMap<>();
map.put(1,"one");
map.put(2,"two");
map.put(3,"three");

for(...)
{
		... //遍历，结果为：1=>one, 2=>two, 3=>three
}
```

## 1.9 TreeMap

TreeMap基于红黑树实现。映射根据Key的自然顺序排序，或者根据创建映射时提供的Comparator进行排序。

TreeMap的基本操作containsKey、get、put、remove的时间复杂度为`O(logN)`

TreeMap重要的成员变量

- root
    - 红黑树的根节点，Entry类型
    - Entry是红黑树的节点，根据key排序，包含的内容是value，比较大小是根据comparator来判断，size是红黑树的节点个数
        - key
        - value
        - left
        - right
        - parent
        - color
- size
    - 红黑树节点个数
- comparator

## 1.10 ArrayList和LinkedList

ArrayList

- 实现基于数组
- 随机访问元素时间复杂度O(1)
- 插入和删除，比LinkedList慢，因为ArrayList需要重新计算大小或者更新索引

LinkedList

- 实现基于双向链表
- 随机访问元素时间复杂度O(N)
- LinkedList更占内存，因为LinkedList的节点除了存储数据，还存储了两个引用（prev，next）

## ArrayList的数据结构

底层由数组实现，默认第一次插入元素时创建大小为10的数组，超出限制时会增加50%的容量。

数据以`System.arraycopy()`复制到新的数组。

按数组下标访问元素性能很高，和数组一样，直接在末尾加入元素性能也高。但是如果按下标插入和删除元素，因为需要移动元素，性能就差了

## 1.11 CopyOnWriteArrayList（读写分离）

Java并发包里提供的并发类，是一个线程安全且读写无锁的ArrayList。

在写操作时，会复制一份新的List，在新的List上完成写操作，这时候写操作时上锁的，然后再将原引用指向新的List，如此保证线程安全。

允许线程并发访问读操作，没有加锁限制，性能较高。如果在写操作过程中，需要读操作，那么会在原容器上读。

## 1.12 TreeSet和HashSet的区别

HashSet和TreeSet的元素都是不能重复的，并且都是线程不安全的

区别

1. HashSet中的元素可以是null，TreeSet中不行
2. HashSet不能保证元素的排列顺序，TreeSet支持自然排序和定制排序
3. HashSet底层采用哈希表实现，TreeSet底层采用红黑树实现

## 1.13 HashSet

基于HashMap实现，默认构造函数是初始容量16，负载因子0.75的HashMap。

封装了一个HashMap对象存储所有的集合元素，所有放入HashSet的元素实际上由HashMap的key来保存，而value则存储了一个PRESENT，静态的Object对象

## 1.14 BlockingQueue

Java并发包里的线程安全的Queue的接口，实现类有ArrayBlockingQueue, DelayQueue, LinkedBlockingQueue, PriorityBlockingQueue, SynchronousQueue等。

主要是为了在多线程环境中支持队列的生产者-消费者操作

### BlockingQueue的方法

|  | 抛异常 | 特定值 | 阻塞 | 超时 |
| --- | --- | --- | --- | --- |
| 插入 | add(e) | offer(e) | put(e) | offer(e, time, unit) |
| 移除 | remove() | poll() | take() | poll(time, unit) |
| 检查 | element() | peek() |  |  |

抛异常：如果操作无法立即执行，则抛出一个异常

特定值：如果操作无法立即执行，则返回一个特定的值（一般是true/false）

阻塞：如果操作无法立即执行，则该方法调用将会发生阻塞，直到能够执行

超时：如果操作无法立即执行，则该方法调用将会发生阻塞，直到能够执行，但等待时间不会超过给定值，并返回一个特定值以告知该操作是否成功（典型的是true/false）

## 1.15 Stream

Stream 是一个核心的功能，它代表了数据流的抽象，并且支持多种操作，这些操作可以是中间操作（返回流本身的操作）和终端操作（返回结果或者副作用的操作）。

主要方法类别

- 创建流
    - Stream.of：从一组元素创建流。
    - Stream.empty：创建一个空流。
    - Arrays.stream：从数组创建流。
    - Collection.stream：从集合创建流。
- 中间操作（Intermediate Operations）中间操作返回流，允许多个操作串联在一起。
    - filter：根据条件过滤元素。
    - map：将流中的元素映射为其他形式。
    - flatMap：将流中的每个元素转换为其他对象的流，并将所有流连接成一个流。
    - distinct：去除重复元素。
    - sorted：对流中的元素进行排序。
    - peek：产生副作用，如打印每个元素。
    - limit：限制流中元素的数量。
    - skip：跳过前n个元素。
- 终端操作（Terminal Operations）终端操作返回结果或者有副作用。
    - forEach：对流中的每个元素执行操作。
    - forEachOrdered：按照流中元素的顺序执行操作。
    - toArray：将流转换为数组。
    - reduce：将流中的元素组合起来，使用提供的累积函数，返回一个可选的结果。
    - collect：将流转换为其他形式，如集合。
    - min：返回流中的最小元素。
    - max：返回流中的最大元素。
    - count：返回流中的元素数量。
    - anyMatch：检查流中是否至少有一个元素匹配给定的谓词。
    - allMatch：检查流中的元素是否都匹配给定的谓词。
    - noneMatch：检查流中的元素是否都不匹配给定的谓词。
    - findFirst：返回流中的第一个元素。
    - findAny：返回流中的任意一个元素。
- 并行流
    - parallel：将流转换为并行流。
    - sequential：将流转换为顺序流。