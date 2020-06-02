#### ArrayList ####

ArrayLsit 有三种初始化方法： 无参数初始化、指定大小初始化、指定数据初始化

源码的中真正保存数据的是transient Object[] elementData;

在源码中定义了两个空的Object数组

````
private static final Object[] EMPTY_ELEMENTDATA = {};
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
````

当通过无参数初始化时，elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA；

当通过指定大小初始化或指定数据初始化时，若指定的大小或指定的数组等于0时，elementData = EMPTY_ELEMENTDATA；

为什么不同初始化方法下，数据为空时候，elementData指定给不同的空对象？

原因是自动扩容的时候，如果是无参数初始化，会用默认的10进行扩容。所以我理解为 两个Object数组实际是想记录初始化的方式。



新增和扩容

新增分为两步。1判断是否需要扩容，若需要则执行扩容。2直接赋值

扩容：

1.先判断是否是无参数初始化。是则当前期望的最小容量minCapacity为默认的10

 if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {

  minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);

 }

2.判断当前数组长度大于minCapacity。是则执行扩容。

3.先进行一次扩容。将现有数组大小扩容至1.5倍。

 int oldCapacity = elementData.length;

 int newCapacity = oldCapacity + (oldCapacity >> 1);

4.扩容后的值newCapacity大于期望值minCapacity，则使用newCapacity进行后续的数组复制 newCapacity = minCapacity;

若扩容后newCapacity仍小于期望值minCapacity，直接将期望值minCapacity用于后续的数组复制。newCapacity = minCapacity;

5.如果扩容后的值大于jvm所能分配给数组的最大值，则使用Integer的最大值

````
 if (newCapacity - MAX_ARRAY_SIZE > 0)

  newCapacity = hugeCapacity(minCapacity);
````

6.通过复制进行扩容
elementData = Arrays.copyOf(elementData, newCapacity);

扩容的本质是 构建一个符合预期大小的数组，再将老数组的数据拷贝到新数组。真正拷贝用的是Native方法 System.arraycopy

删除
remove可以根据索引、值进行删除。删除的本质是进行了数组的拷贝。


####  LinkList ####
适用于集合元素先入先出（队列）和先入后出（栈）。

底层数据结构是一个双向链表。链表每个元素叫做Node，有三个部分组成：item（本身节点的值）、next（指向下一个节点）、prev（指向上一个节点）

first 是双向链表的头结点，它的prev为null，

last是双向链表的尾节点，它的next为null。

当链表为空，first和last 是同一个节点。前后都是null。

因为是双向链表，只要机器内存足够大，理论上是没有大小限制。

常用方法：
add：从尾部追加节点。 1 新增一个A尾节点prev为last，值为e，next为null。2 last等于新的尾节点A。3 lase的next指向A
addFirst：从头部追加

删除同样可以从头部或者尾部删除。

节点查询
通过指定索引index查找元素。查找会通过简单的二分法查找。当index位于前半部分从first开始，否则从last开始。

迭代器
 LinkedList 要实现双向的迭代访问，使用 Iterator 接口肯定不行，因为 Iterator 只支持从头到尾的访问。Java 新增了一个迭代接口，叫做：ListIterator，这个接口提供了向前和向后的迭代方法。

从尾到头：hasPrevious、previous、previousIndex

从头到尾:  hasNext、next、nextIndex


LinkList还实现了 Queue、Deque接口。这两个接口都提供 新增、删除、查找 从头或者从尾开始。当链表为空时，前者返回特殊值。后者和原生的一样都抛出异常，
