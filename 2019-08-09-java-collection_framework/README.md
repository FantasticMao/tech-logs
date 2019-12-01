# Java 集合框架学习

| List                     | Set                     | Map                   | Queue               |
| ------------------------ | ----------------------- | --------------------- | ------------------- |
| ArrayList                | HashSet                 | HashMap               | ArrayBlockingQueue  |
| LinkedList               | LinkedHashSet           | LinkedHashMap         | LinkedBlockingQueue |
| **CopyOnWriteArrayList** | TreeSet                 | TreeMap               |
| **Vector**               | **CopyOnWriteArraySet** | **Hashtable**         |
| **Stack**                |                         | **ConcurrentHashMap** |
|                          |                         | WeakHashMap           |

## ArrayList

- 内部使用 `Object[]` 保存元素
- 实现了 `RandomAccess` 接口
- 新扩容的数组长度 = 旧数组长度 + 旧数组长度 / 2
- 查询元素 O(1)，插入元素 O(n)
- 源码走读 https://gist.github.com/FantasticMao/68258d17fe0dead18b816b5a750ba28c

## LinkedList

- 使用双向链表保存内部元素
- 实现了 `Deque` 接口
- 查询元素 O(n)，插入元素 O(1)
- 源码走读 https://gist.github.com/FantasticMao/75b6cb284ff1dfd488627d83498585d8

## CopyOnWriteArrayList

- 内部使用 `Object[]` 保存元素
- 使用 `volatile` 修饰 `Object[]`：保证查询元素的可见性
- 实现了 `RandomAccess` 接口
- 使用 `ReentrantLock` 同步：修改元素时会锁定，查询元素时不会锁定

## Vector

- 内部使用 `Object[]` 保存元素
- 实现了 `RandomAccess` 接口
- 使用 `synchronized` 同步：对内部元素的所有操作都会锁定

## Stack

- 继承 `Vector`
- 实现 LIFO 操作：Last In，First Out

---

## HashSet

- 基于 `HashMap` 实现，Key 是 `Set` 集合元素，Value 是 `new Object()` 对象
- 不能保证顺序

## LinkedHashSet

- 基于 `LinkedHashMap` 实现，Key 是 `Set` 集合元素，Value 是 `new Object()` 对象
- 可以保证插入顺序

## TreeSet

- 基于 `TreeMap` 实现
- 支持排序操作

## CopyOnWriteArraySet

- 基于 `CopyOnWriteArrayList` 实现

---

## HashMap

- 内部使用节点数组+链表/红黑树保存元素
- 使用 `(table.lenth - 1) & hashCode` 计算元素在数组内的下标位置
- 当链表长度大于 8 的时候，JDK8 中会讲链表优化为红黑树
- 源码走读 https://gist.github.com/FantasticMao/9c088055dab10cdd1247e007bde6c7e5

## LinkedHashMap

- 基于 `HashMap` 实现，继承重写 `HashMap.Node` 类为 **双向链表**
- 可以保证插入顺序
- 源码走读 https://gist.github.com/FantasticMao/2042aa3a1c62c02f734109a4081f9f43

## TreeMap

- 基于红黑树算法实现
- 支持排序操作

> 红黑树是一种自平衡的二叉查找树，性质如下：
>
> 1. 节点是红色或者黑色
> 2. 根节点是黑色
> 3. 每个叶子节点都是黑色
> 4. 每个红色节点必须连接两个黑色子节点
> 5. 任意节点到它的叶子节点的所有路径都包含相同数目的黑色节点

## Hashtable

- 内部使用节点数组 + 链表保存元素
- 使用 `(hash & 0x7FFFFFFF) % tab.length` 计算元素在数组内的下标位置
- 不允许 key == null
- 使用 `synchronized` 同步：对内部元素的所有操作都会锁定
- 源码走读 https://gist.github.com/FantasticMao/77a9752014c1e05860840e711627d474

## ConcurrentHashMap

- 内部的数据结构和算法和 `HashMap` 类似：使用节点数组+链表/红黑树保存元素，使用 `(table.lenth - 1) & hashCode` 计算元素在数组内的下标位置
- 所用 CAS 同步节点数组，使用 `synchronized` 同步链表/红黑树：添加元素时会锁定，读取元素时不会锁定

## WeakHashMap

- 内部使用 `WeakReference` 节点数组+链表保存元素
- 使用 `(table.lenth - 1) & hashCode` 计算元素在数组内的下标位置

> Java 引用类型：
>
> 1. 强引用就是指程序代码之中普遍存在的，类似 `Object obj = new Object()` 这类的引用。只要强引用还存在，垃圾收集器就永远不会回收被引用的对象。
> 2. 软引用是用来描述一些还有用但并非必需的对象。对于软引用关联着的对象，在系统将要发生内存溢出异常之前，将会把这些对象列进回收范围之中，进行第二次回收。如果这次回收还没有足够的内存，才会抛出内存溢出异常。
> 3. 弱引用也是用来描述非必需的对象，但是它的强度比软引用更弱一些，被弱引用关联的对象只能生存到下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。
> 4. 虚引用也称为幽灵引用或幻影引用，是最弱的一种引用关系。一个对象是否有虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个对象实例。为一个对象设置虚引用关联的唯一目的，就是能在这个对象被垃圾收集器回收时，收到一个系统通知。

---

## ArrayBlockingQueue

- 有界阻塞队列，基于数组实现
- 内部使用 `ReentrantLock` 和 `Condition` 实现阻塞逻辑

## LinkedBlockingQueue

- 无界阻塞队列，基于链表实现
- 内部使用 `ReentrantLock` 和 `Condition` 实现阻塞逻辑
