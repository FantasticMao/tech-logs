# Unsafe

`sun.misc.Unsafe` 是 Java 语言中用于执行低级别、不安全操作的的类，Java 类库中很多高阶操作都是通过 `Unsafe` 实现的，例如 `java.util.concurrent.atomic.AtomicInteger`、`java.util.concurrent.locks.AbstractQueuedSynchronizer`、`java.nio.DirectByteBuffer` 等等。下面记录 `Unsafe` 类中的部分 API。

---

## 内存相关操作

### 根据偏移量，获取/设置对象字段

- **Object 类型** `getObject(Object o, long offset)`、`putObject(Object o, long offset, Object x)`
- **int 类型** `getInt(Object o, long offset)`、`putInt(Object o, long offset, int x)`
- **long 类型** `getLong(Object o, long offset)`、`putLong(Object o, long offset, long x)`
- **其它类型 ......**

### 根据偏移量，保证可见性和有序性（即 `volatile` 语义）地获取/设置对象字段

- **Object 类型** `getObjectVolatile(Object o, long offset)`、`putObjectVolatile(Object o, long offset, Object x)`
- **int 类型** `getIntVolatile(Object o, long offset)`、`putIntVolatile(Object o, long offset, int x)`
- **long 类型** `getLongVolatile(Object o, long offset)`、`putLongVolatile(Object o, long offset, long x)`

### 根据偏移量，保证有序性地获取/设置对象字段

- **Object 类型** `putOrderedObject(Object o, long offset, Object x)`
- **int 类型** `putOrderedInt(Object o, long offset, int x)`
- **long 类型** `putOrderedLong(Object o, long offset, long x)`

### 直接操作内存

- **根据内存地址，获取/设置变量指针** `getAddress(long address)`、`putAddress(long address, long x)`
- **分配内存** `allocateMemory(long bytes)`
- **扩充内存** `reallocateMemory(long address, long bytes)`
- **在给定内存块中设值** `setMemory(Object o, long offset, long bytes, byte value)`
- **拷贝内存** `copyMemory(Object srcBase, long srcOffset, Object destBase, long destOffset, long bytes)`
- **释放内存** `freeMemory(long address)`

---

## 类/对象相关操作

### 获取 field 内存地址偏移量

- **获取静态字段偏移量** `staticFieldOffset(Field f)`
- **获取实例字段偏移量** `objectFieldOffset(Field f)`
- **获取静态字段对应的对象指针** `staticFieldBase(Field f)`

### 类相关操作

- **判断一个类是否需要初始化** `shouldBeInitialized`
- **确认一个类是否已经初始化** `ensureClassInitialized(Class c)`
- **定义一个类** `defineClass(String name, byte[] b, int off, int len, ClassLoader loader, ProtectionDomain protectionDomain)`
- **定义一个匿名类** `defineAnonymousClass(Class hostClass, byte[] data, Object[] cpPatches)`

### 对象相关操作

- **创建对象（不会调用构造方法）** `allocateInstance(Class cls)`
- **获取对象锁** `monitorEnter(Object o)`
- **释放对象锁** `monitorExit(Object o)`
- **尝试获取对象锁** `tryMonitorEnter(Object o)`
- **抛出异常** `throwException(Throwable ee)`

### 数组相关操作

- **获取数组第一个元素的偏移量** `arrayBaseOffset(Class arrayClass)`
- **获取数组元素占用的字节大小** `arrayIndexScale(Class arrayClass)`

---

## 操作系统相关

### 获取系统指针和内存页大小

- **获取系统指针占用的字节大小** `addressSize()`
- **获取内存页占用的字节大小** `pageSize()`
- **获取系统负载情况（相当于 Linux 中的 uptime）** `getLoadAverage(double[] loadavg, int nelems)`

### CAS 相关操作

- **Object 类型** `compareAndSwapObject(Object o, long offset, Object expected, Object x)`
- **int 类型** `compareAndSwapInt(Object o, long offset, int expected, int x)`
- **long 类型** `compareAndSwapLong(Object o, long offset, long expected, long x);`

### 线程调度

- **恢复线程** `unpark(Object thread)`
- **挂起线程** `park(boolean isAbsolute, long time)`

### 内存屏障相关操作

- **禁止 load 操作重排序** `loadFence()`
- **禁止 store 操作重排序** `storeFence()`
- **禁止 load、store 操作重排序** `fullFence()`

---

参考资料：

- [OpenJDK Unsafe 源码](http://hg.openjdk.java.net/jdk8/jdk8/jdk/file/687fd7c7986d/src/share/classes/sun/misc/Unsafe.java)
- [Java 魔法类：Unsafe 应用解析](https://tech.meituan.com/2019/02/14/talk-about-java-magic-class-unsafe.html)
- [man uptime](https://man.linuxde.net/uptime)
