### 集合的特点

集合的特点主要有如下两点：

- 对象封装数据，对象多了也需要存储。集合用于存储对象。
- 对象的个数确定可以使用数组，对象的个数不确定的可以用集合。因为集合是可变长度的。

### 集合和数组的区别

- 数组是固定长度的；集合可变长度的。
- 数组可以存储基本数据类型，也可以存储引用数据类型；集合只能存储引用数据类型。
- 数组存储的元素必须是同一个数据类型；集合存储的对象可以是不同数据类型。

### 集合框架底层数据结构

Collection

​		List

- Arraylist： Object数组

- Vector： Object数组

- LinkedList： 双向循环链表

  Set

- HashSet（无序，唯一）：基于 HashMap 实现的，底层采用 HashMap 来保存元素

- LinkedHashSet： LinkedHashSet 继承与 HashSet，并且其内部是通过 LinkedHashMap 来实现的。有点类似于我们之前说的LinkedHashMap 其内部是基于 Hashmap 实现一样，不过还是有一点点区别的。

- TreeSet（有序，唯一）： 红黑树(自平衡的排序二叉树。)



Map

- HashMap： JDK1.8之前HashMap由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）.JDK1.8以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间
- LinkedHashMap：LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外,LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。

- HashTable： 数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的

- TreeMap： 红黑树（自平衡的排序二叉树）





### 哪些集合类是线程安全的？

- vector：就比arraylist多了个同步化机制（线程安全），因为效率较低，现在已经不太建议使用。在web应用中，特别是前台页面，往往效率（页面响应速度）是优先考虑的。

- statck：堆栈类，先进后出。
- hashtable：就比hashmap多了个线程安全。

- enumeration：枚举，相当于迭代器。



### Java集合的快速失败机制 “fail-fast”？

​		是java集合的一种错误检测机制，当多个线程对集合进行结构上的改变的操作时，有可能会产生 fail-fast 机制。

​		例如：假设存在两个线程（线程1、线程2），线程1通过Iterator在遍历集合A中的元素，在某个时候线程2修改了集合A的结构（是结构上面的修改，而不是简单的修改集合元素的内容），那么这个时候程序就会抛出 ConcurrentModificationException 异常，从而产生fail-fast机制。

​		原因：迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个 modCount 变量。集合在被遍历期间如果内容发生变化，就会改变modCount的值。每当迭代器使用hashNext()/next()遍历下一个元素之前，都会检测modCount变量是否为expectedmodCount值，是的话就返回遍历；否则抛出异常，终止遍历。



解决办法：

1. 在遍历过程中，所有涉及到改变modCount值得地方全部加上synchronized。
2. 使用CopyOnWriteArrayList来替换ArrayList



#### 遍历一个 List 有哪些不同的方式？每种方法的实现原理是什么？Java 中 List 遍历的最佳实践是什么？

​		最佳实践：Java Collections 框架中提供了一个 RandomAccess 接口，用来标记 List 实现是否支持 Random Access。

​		如果一个数据集合实现了该接口，就意味着它支持 Random Access，按位置读取元素的平均时间复杂度为 O(1)，如ArrayList。
​		如果没有实现该接口，表示不支持 Random Access，如LinkedList。



#### 说一下 ArrayList 的优缺点

ArrayList的优点如下：

​		ArrayList 底层以数组实现，是一种随机访问模式。ArrayList 实现了 RandomAccess 接口，因此查找的时候非常快。
​		ArrayList 在顺序添加一个元素的时候非常方便。
ArrayList 的缺点如下：

​		删除元素的时候，需要做一次元素复制操作。如果要复制的元素很多，那么就会比较耗费性能。
​		插入元素的时候，也需要做一次元素复制操作，缺点同上。

ArrayList 比较适合顺序添加、随机访问的场景。





#### ArrayList 和 LinkedList 的区别是什么？

​		数据结构实现：ArrayList 是动态数组的数据结构实现，而 LinkedList 是双向链表的数据结构实现。
​		随机访问效率：ArrayList 比 LinkedList 在随机访问的时候效率要高，因为 LinkedList 是线性的数据存储方式，所以需要移动指针从前往后依次查找。
​		增加和删除效率：在非首尾的增加和删除操作，LinkedList 要比 ArrayList 效率要高，因为 ArrayList 增删操作要影响数组内的其他数据的下标。
​		内存空间占用：LinkedList 比 ArrayList 更占内存，因为 LinkedList 的节点除了存储数据，还存储了两个引用，一个指向前一个元素，一个指向后一个元素。
​		线程安全：ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全；

综合来说，在需要频繁读取集合中的元素时，更推荐使用 ArrayList，而在插入和删除操作较多时，更推荐使用 LinkedList。



#### ArrayList 和 Vector 的区别是什么？

这两个类都实现了 List 接口（List 接口继承了 Collection 接口），他们都是有序集合

​		线程安全：Vector 使用了 Synchronized 来实现线程同步，是线程安全的，而 ArrayList 是非线程安全的。
​		性能：ArrayList 在性能方面要优于 Vector。
​		扩容：ArrayList 和 Vector 都会根据实际的需要动态的调整容量，只不过在 Vector 扩容每次会增加 1 倍，而 ArrayList 只会增加 50%。

​		Vector类的所有方法都是同步的。可以由两个线程安全地访问一个Vector对象、但是一个线程访问Vector的话代码要在同步操作上耗费大量的时间。

​		Arraylist不是同步的，所以在不需要保证线程安全时时建议使用Arraylist。



#### List 和 Set 的区别

​		Set：检索元素效率低下，删除和插入效率高，插入和删除不会引起元素位置改变。
​		List：和数组类似，List可以动态增长，查找元素效率高，插入删除元素效率低，因为会引起其他元素位置改变





## Map接口

### 说一下 HashMap 的实现原理？

### HashMap在JDK1.7和JDK1.8中有哪些不同？HashMap的底层实现



### 如何决定使用 HashMap 还是 TreeMap？

​		对于在Map中插入、删除和定位元素这类操作，HashMap是最好的选择。然而，假如你需要对一个有序的key集合进行遍历，TreeMap是更好的选择。基于你的collection的大小，也许向HashMap中添加元素会更快，将map换为TreeMap进行有序key的遍历。